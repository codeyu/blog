---
title: linux zsh 使用 sudo 执行 bash 脚本提示 command not found
date: 2016-09-22 21:58:03
categories: 翻译
tags: [linux, bash]
copyright: false
---
<!--more-->

## 问[^1]

在 home 目录，我有一个脚本叫 `foo.sh`。我切换到此目录，执行 `./foo.sh`，发生一个错误：
```
    zsh: permission denied: ./foo.sh
```
我用 `sudo ./foo.sh` 继续执行，发生另一个错误：
```
    sudo: ./foo.sh: command not found
```
什么情况？我该怎么做？

## 答

### permission denied

为了运行一个脚本文件，此文件必须有**一个可执行权限设置位**

为了完全理解 Linux 文件权限，通过[这个文档](http://www.gnu.org/software/coreutils/manual/html_node/File-permissions.html#File-permissions)你可以学习 `chmod` 命令。[chmod](http://www.gnu.org/software/coreutils/manual/html_node/chmod-invocation.html)，change mode 的缩写形式，是改变文件权限设置的一个命令。

在你的系统运行 `man chmod` 或 `info chmod` 可以查看 chmod 的帮助说明。学习了解后，你就能看懂下面的命令输出的结果：
```
ls －l foo.sh
```
结果将列出 Owner，Group，Other（有时称之为“World”或“Other”）的 **可读**，**可写**，**可执行** 权限。

下面简单说下怎么去解决你遇到的 **Permission Denied error**。

```
$ ls -l foo.sh                    # 查看 foo.sh 文件的权限
-rw-r--r-- 1 rkielty users 0 2012-10-21 14:47 foo.sh 
    ^^^ 
 ^^^ | ^^^   ^^^^^^^ ^^^^^
  |  |  |       |       | 
Owner| World    |       |
     |          |    Name of
   Group        |     Group
             Name of 
              Owner 
```
上面的输出表明 Owner 有读和写权限，但 - 表示没有可执行权限（Group 和 World 只有读权限）。

现在用 `chmod` 命令修正之。

```
$ chmod +x foo.sh               # owner 可以设置 foo.sh 的可执行权限
$ ls -l foo.sh                  # 现在我们看到在 rw 后有个 x
-rwxr-xr-x 1 rkielty users 0 2012-10-21 14:47 foo.sh
   ^  ^  ^
```
在 Linux 中，现在 foo.sh 是可执行的。

### 用 sudo 结果显示 Command not found

当你在执行命令前用 **sudo** 实际上你是作为超级用户或 root 用户执行它。

root 用户找不到你的命令的原因可能是 root 的 `PATH` 环境变量不包括 `foo.sh` 文件所在的位置。因此你命令未找到。

PATH 环境变量包含一个搜索命令的目录列表。每个用户根据自己的情况设置 PATH 变量。执行下面的命令看看 PATH 变量里都有些什么：

```
env | grep ^PATH
```
下面是一些示例输出，首先作为一个普通用户运行上面的 env 命令，然后使用 sudo 作为 root 用户运行的：
```
rkielty@rkielty-laptop:~$ env | grep ^PATH
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games

rkielty@rkielty-laptop:~$ sudo env | grep ^PATH
[sudo] password for rkielty: 
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/X11R6/bin
```
请注意：尽管相似，但非特权用户（rkielty）和 root 用户的 PATH 目录列表是不一样的。

foo.sh 所在的目录没有出现在 root 用户的 PATH 变量中，因此 `command not found` 错误出现了。

[^1]: 原文地址：[Command not found when using sudo](http://stackoverflow.com/questions/12996397/command-not-found-when-using-sudo)