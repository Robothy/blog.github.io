---
layout: post
author: 罗福享
title: Cron 用法
#date: 2018.06.21 01:57:32 +0800
categories: coding
tags: Cron, Linux
---

Cron 是类 Unix 操作系统上基于时间的任务计划程序，通常使用 Cron 这一工具在 Linux 操作系统下**定期执行**某些任务（命令或脚本）。

## Cron 配置文件

Cron 根据配置文件的内容定期执行相应的任务，它的配置文件是 `/etc/crontab`, 在没有修改配置文件的情况下，执行

```bash
vi /ect/crontab
```

我们可以看到如下内容

```bash
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user  command
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )

```

其中 `SHELL=/bin/sh` 指明执行shell的命令是 `/bin/sh`，有些Linux发行版为 `/bin/bash`，具体值依据操作系统来定。

`PATH` 指名命令所在路径，若没有指明此参数，Cron将找不到某些命令。

cron 在用户登录操作系统时从 `/etc/init.d` 中自动启动，因此我们在进入 Linux 操作系统之后不需要手动启动相关服务。

Crontab 是 Cron Table 的缩写，使用 `crontab` 命令能够对 Cron 进行配置，即创建定时任务。
