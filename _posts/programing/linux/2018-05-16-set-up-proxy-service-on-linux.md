---
layout: post
categories: programing linux squid
title: "在 Linux 操作系统中使用 Squid 配置代理服务"
---

本文将一步步描述如何使用 Squid 在 Linux 操作系统(Ubuntu 16.04 64bit)中配置代理服务，实验中使用了两台服务器，一台配置为代理服务器 (Proxy Server)，另一台作为提供 Web 服务的服务器 (Web Server)。

## 测试程序准备

在 Web 服务其中编写一个测试程序，模拟 Web 服务，当用户通过浏览器向该 Web 服务发送请求时，服务器返回客户机的IP地址并显示在浏览器中。这里本人使用 golang 编写了这个测试程序，读者可使用自己熟悉的语言实现测试程序，测试程序源码如下：

```go
package main

import (
        "fmt"
        "log"
        "net/http"
        "os"
)

func main() {
        http.HandleFunc("/visit", handleVisit)
        err := http.ListenAndServe(":6060", nil)
        if nil != err {
                log.Fatal("ListenAndServer: ", err)
        }
}

var visitTimes int = 0


func handleVisit(w http.ResponseWriter, r *http.Request) {
        host := r.RemoteAddr
        fmt.Fprintf(w, host)
        visitTimes++
        if 10 <= visitTimes {
                os.Exit(0)
        }
}

```

使用浏览器访问该测试程序，效果如下图所示。

![图片未加载](https://gitee.com/robothy/img/raw/master/luofuxiang/764971-20180322110325392-1781004861.jpg "使用PC机访问测试程序效果")

搭建好代理服务器之后，只要通过代理服务器方位测试程序，能够在页面上显示代理服务器的IP地址，则说明代理服务器配置成功！

## 安装 Squid

Squid 的安装比较简单，打开控制台，使用 apt-get 命令即可，若当前登录用户非 root 用户，需要在命令前面加上 `sudo`。

```bash
root@iZwz9aehdis8zy914onlasZ:~# apt-get install squid3
```

安装完成之后，使用 `squid -v`命令可查看 Squid 的版本号，本实验安装的 squid 版本是3.5.12。

## 配置 Squid

Squid 的配置文件是 `/etc/squid/squid.conf`，这个文件较长，大约有七千多行。在正常使用 Squid 之前，需要进行如下简单配置：

+ 取消配置文件中 `http_access allow localnet` 这行的注释，允许本地网络走代理。

+ 将 `http_access deny all` 修改为 `http_access allow all` ， 允许所有的客户端走该代理，*此操作可能导致机器被利用*，但为方便实验，暂时这样设置。


>在Linux系统中修改该配置文件的时候比较难找到配置所在行，可以使用 `grep -n 'http_access' /etc/squid/squid.conf`查看配置所在的行，然后用 vi 打开配置文件，在命令模式下输入`:行号`，可以定位到该行。

Squid 大部分配置的说明在配置文件中都有注释说明，也可以在 Squid [官网]("http://www.squid-cache.org/Doc/config/" "Squid 配置说明")找到相应的说明。

## Squid 服务的启动与停止

squid 安装的时候就已经自动配置成了服务， squid 服务的启停与一般的Linux服务的启停类似。
**启动服务**

```bash
root@iZwz9aehdis8zy914onlasZ:~# service squid start
```

**停止服务**

```bash
root@iZwz9aehdis8zy914onlasZ:~# service squid stop
```

**重新启动服务**

```bash
root@iZwz9aehdis8zy914onlasZ:~# service squid restart
```

修改好配置文件之后，`service squid start`启动squid 服务，启动完成之后，`tail -f /var/log/squid/access.log`，监控 squid 的日志文件。

## 验证

最后一步是通过Chrome浏览器验证代理服务是否有效。

+ 打开 Chrome 浏览器，设置

+ 打开代理设置

+ 局域网设置

+ 勾选"为LAN使用代理服务器（这些设置不用于拨号或VPN连接）"

+ 填入代理服务器的IP地址和端口号，squid的默认端口号是3128

设置完成之后，访问测试程序，测试程序返回了代理服务器的IP地址，说明代理服务器配置成功。

![图片未加载](https://gitee.com/robothy/img/raw/master/luofuxiang/764971-20180322110516854-1965713767.jpg "测试程序返回代理服务器IP地址")

同时监控 squid 日志文件的控制台打印出日志：

``` bash
1521628949.607     18 116.226.242.24 TCP_MISS/200 268 GET http://luofuxiang.site:6060/visit - HIER_DIRECT/139.199.156.122 text/plain
```