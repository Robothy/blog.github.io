---
layout: post
categories: programing chrom-extension
title: "Chrome Extension 的消息传递"
---
# Chrome Extension 的消息传递

在开发 Chrome Extension 的过程中，Extension 各个部分之间经常需要进行消息传递，Chrome Extension 通信机制允许 Content Scripts 与 Background Page 之间，Extension 与其它 Extension 之间(前提是知道其它 Extension 的 ID)进行通信。通信双方之间均可以发送消息或者监听消息。一条消息可以包含任意合法的 JSON 对象(null, boolean, number, string, array, 或者 object)。通信的方式可以是发送单次请求(one-time request)，也可以建立一个长时间活动的连接(long-lived connections)。
> 若要实现不同 Content Scripts 之间的消息传递，应借助 Background Page 进行转发。

![图片未加载](123.231 "Chrome Extension 消息传递")

## 通过单次消息请求传递消息

若通信双方每次仅需要传递一条数据，则可以使用单次请求的方式传递消息。下面这个例子实现了 Content Scripts 与 Background Scripts 之间以单次请求方式传递消息。

首先创建文件夹**message_passing**, 在文件夹中创建**manifest.json**, manifest 文件中的内容如下:

```json
{
  "name": "Say Hello.",
  "description": "new page say hello to backgroun page.",
  "version": "1.0",
  "content_scripts":[{
    "js":["sayhello.js"],
    "matches":["https://*/*", "http://*/*"],
    "run_at":"document_start"
  }],
  "background":{
    "scripts": ["background.js"],
    "persistent": false
  },
  "manifest_version": 2
}
```

`content_script` 指明注入到页面中的脚本名称为**sayhello.js**，注入的页面的所有`http`和`https`页面。因此，在 Chrome 浏览器中每打开一个新的页面，**sayhello.js**都会被加载。

然后创建**sayhello.js**, 文件内容如下：

```JavaScript
// 发送消息，每次打开页面的时候发送一条问候消息
chrome.runtime.sendMessage({from: window.location.hostname, 
    requestMessage: "hello, this is " + window.location.hostname}, 
    function(response) {
    if(undefined != response){
        alert(response.from + ": " + response.responseMessage)
    }
})
```

每打一个新的页面，注入到页面的Content Script都会通过`sendMessage`发送一条消息。若发出的消息得到相应，则让响应的消息在弹出框中显示。

> 这里需要注意的是，若发送者的消息被多个监听者接受并相应，发送者只能接收到第一个监听者的响应。Chrome 官方文档原文是：If multiple pages are listening for onMessage events, only the first to call sendResponse() for a particular event will succeed in sending the response. All other responses to that event will be ignored.

最后创建 **background.js**，文件内容如下：

``` JavaScript
// 设置监听，接收消息，并回复
chrome.runtime.onMessage.addListener(
    function(request, sender, sendResponse) {
      console.log(request.from + ": " + request.requestMessage);
      sendResponse({from:"background",
        responseMessage:"Welcome, " + request.from});
});
```

background script 监听 Content Scripts 发来的消息，每次监听到消息都作出一次相应。

将 Extension 加载到浏览器当中，打开 http://www.luofuxiang.site， 注入到页面的 content script 将向 background page 发送问候信息，background page 监听到问候消息时作出响应，content script 将响应的信息弹出。本实验 Chrome Extension 的完整源码可以[点击这里](https://pan.baidu.com/s/1lwt0ucbeAEz2V9PM5OIBFg)`密码:b9bx`下载。

## 通过长时间活动连接的方式传递消息

