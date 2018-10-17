---
layout: post
categories: programing eclipse
title: "eclipse代码自动补全设置"
---
# eclipse代码自动补全设置

## 说明

eclipse安装好了之后，在编辑框中输入某个英文字符，默认不自动弹出自动代码选择框，需要手动按下 <kbd>Alt</kbd> + <kbd>/</kbd>或者输入的字符为 `.`  才弹出代码自动补全框。其实eclipse是可以设置成向Visual Studio那样弹出代码自动补全框，供程序员选择要输入的字符串。自动选择要输入的字符串除了能够保证输入的便捷性与准确性之外，还可以让eclipse自动导入要使用到的包。

## 步骤

### 设置Auto Activation参数值

从eclipse菜单栏依次进入 `Window 》 Preferences` ，出现如图2.1所示对话框，选中 Java 》 Editor 》 Content Assist ，找到 `Auto Activation` ，可以看到有`Auto activation delay(ms)` 和 `Auto activation triggles for Java`两个参数，这两个参数分别表示代码自动补全框触发时间与触发字符。触发时间一般默认为200ms，这里将其改为20ms或者更小，不然手动都输完了代码补全框还没有弹出来（补充一下：代码补全框实际弹出时间还与电脑的硬件配置有关）。触发字符当然是26各英文字符大小写都要，默认只有一个 `.`  ，这也可以解释为甚么未经过配置的eclipse输入 `.`会弹出代码补全框。但是，在输入参数的时候我们会发现这里不能输入太多的字符，远远不能满足所有英文字符触发代码补全的需求，可以通过修改配置文件来增加触发字符。这里暂时先输入 `abcd` ，点击 `Apply 》OK` 按钮，保存设置。

![](https://gitee.com/robothy/img/raw/master/luofuxiang/764971-20160930093956078-838627246.png)

图2.1

### 导出配置文件

从eclipse菜单栏进入 `File 》 Export` ，弹出如图2.2所示对话框，选中 `Preferences` ，单击 `Next` 。

![](https://gitee.com/robothy/img/raw/master/luofuxiang/764971-20160930094250594-982880377.png)

图2.2

选择配置文件的导出路径，路径和文件名称可以随意，先记住就行。如图2.3，选中 `Export all` ，选择路径为桌面，给文件随便取了个名字。

![](https://gitee.com/robothy/img/raw/master/luofuxiang/764971-20160930094850500-435133337.png)

图2.3

### 修改配置文件内容

找到上面步骤导出的文件，文件后缀为 `.epf` ，是一个文本文件，选中文件， 右击》编辑 ，使用记事本打开文件。按 <kbd>Ctrl</kbd> + <kbd>F</kbd> ，查找文件中所有的`abcd`字符串，如图，将`abcd`修改为： `.abcefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ` ，保存修改好的配置文件。

![](https://gitee.com/robothy/img/raw/master/luofuxiang/764971-20160930100010360-2050691882.png)
图2.4

### 重新导入配置文件

修改好配置文件之后，我们需要将其重新导入eclipse。导入配置文件的步骤与导出配置文件的步骤类似，从eclipse菜单栏进入 `File》Import` ，选中 `Preferences` ，选择上面步骤修改好的配置文件（如图2.5），单击 `Finish` 。配置原则上可立即生效。

![](https://gitee.com/robothy/img/raw/master/luofuxiang/764971-20160930101112750-1148819120.png)

图2.5
