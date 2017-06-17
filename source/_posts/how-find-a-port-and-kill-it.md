---
title: 如何查找端口占用及杀掉进程
date: 2017-06-17 19:05:59
categories: 技术
tags: [windows, linux]
copyright: false
description: 
---
<!--more-->

### Windows 系统
有一天，我在 Windows 系统启动 Hadoop 服务时，弹出错误提示：10000 端口被占用。

那么，我们来看看如何找到占用 10000 端口的进程并杀掉它。

首先，以管理员身份运行 Powershell，在命令行输入：

```
netstat -aon|findstr "10000"
```
回车，显示如下：
```
TCP    127.0.0.1:10000        0.0.0.0:0              LISTENING       13924
```
上面的输出表示 进程ID为 **13924** 的进程占用了端口10000。

继续查找进程ID为 13924 的进程是什么程序：
```
tasklist|findstr "13924"
```
回车，显示如下：
```
yundetectservice.exe         13924 Console                    1      3,004 K
```
找到了 **yundetectservice.exe** 这个程序。现在杀掉它：
```
taskkill /IM yundetectservice.exe /F
```
或者
```
taskkill /PID 13924 /F
```
现在可以正常启动 Hadoop 服务了。

### Linux 系统
那么，在 Linux 下如何操作呢？使用如下命令：

查找占用端口的进程：
```
netstat -apn|grep <端口号>
```
或者（Mac 系统）
```
lsof –i:<端口号>
```
找到进程号后，使用下面的命令查找程序：
```
ps aux|grep <进程号>
```
杀掉进程有很多方法，如下两种：
```
kill -9 <进程号>
```
或
```
killall <进程名称>
```