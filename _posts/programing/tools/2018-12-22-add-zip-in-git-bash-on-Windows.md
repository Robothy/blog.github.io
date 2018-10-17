---
layout: post
categories: programing git
title: "在 Windows 操作系统下添加 zip 命令到 git bash 中"
---

# 在 Windows 操作系统下添加 zip 命令到 git bash 中

Git 是程序员必备工具之一，在 Windows 下安装 Git 的时候，git bash 会自带很多的 shell 命令，但不包括 `zip` 压缩命令，这里介绍如何在 git bash 中添加 zip 命令。

shell 命令本质上是一个或多个可执行的二进制文件， Windows 下 git bash 中的 `zip` 压缩命令包含 `zip.exe` 和 `bzip2.dll`两个文件，这两个文件需要同时存在于 Git 安装目录下的 `Git/usr/bin` 目录中 `zip` 命令才能正常执行。

可以在[点击这里](http://pan.baidu.com/s/1nuZv8rZ "点击下载 zip.exe 和 bzip2.dll " )（密码:th9l）下载 `zip.exe` 和 `bzip2.dll` 这两个文件。 例如： Git 安装目录为 `D:/Git`， 则这两个文件应该放到 `D:/Git/usr/bin` 目录下。执行 `zip root.zip root.txt` 命令，将 `root.txt` 压缩成 `root.zip`。

以上内容在 `git version 2.13.0.windows.1` 下验证过，读者若在其它环境中往 git bash 添加 `zip` 命令时遇到问题或对本文有建议，可以[给我留言](fuxiang.luo@126.com "给我留言")。