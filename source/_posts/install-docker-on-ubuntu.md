---
title: Ubuntu Trusty 14.04 Docker 安装教程 
date: 2016-09-22 19:47:02
categories: 技术
copyright: false
tags: [ubuntu, docker]
---
<!--more-->
### 准备

docker 要求 64位 的 Linux 操作系统，并且 Linux 的内核 >= 3.10

1. 首先查看你的操作系统的内核版本：
```
$ uname -r
```
2. 更新包管理器 [APT快速参考](http://wiki.ubuntu.com.cn/Apt和dpkg快速参考)
```
$ sudo apt-get update
```
3. 安装推荐的包,这个包可以让 docker 使用 [AUFS](http://coolshell.cn/articles/17061.html) 文件系统
```
$ sudo apt-get install linux-image-extra-$(uname -r) linux-image-extra-virtual
```
4. 更新包管理器并且安装 apt-transport-https 和 ca-certificates
```
 $ sudo apt-get update
 $ sudo apt-get install apt-transport-https ca-certificates
```

5. 添加新的 [GPG](http://www.ruanyifeng.com/blog/2013/07/gpg.html) 键
```
$ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
```
6. 打开 `/etc/apt/sources.list.d/docker.list` 文件，清空内容（如果有）
``` 
 $ sudo vim /etc/apt/sources.list.d/docker.list
```
7. 添加以下内容并保存退出（`!wq`）
``` 
 deb https://apt.dockerproject.org/repo ubuntu-trusty main
```

8. 更新 APT 包索引
```
 $ sudo apt-get update
```
9. 清理旧的 docker repo 源
```
 $ sudo apt-get purge lxc-docker
```
10. 验证 APT 是否从正确的 repo 拉取
```
 $ apt-cache policy docker-engine
```
### 安装

1. 更新 APT 包索引

```
 $ sudo apt-get update
```

2. 安装 docker

```
 $ sudo apt-get install docker-engine
```

3. 启动 docker 守护进程

```
 $ sudo service docker start
```

4. 验证 docker 是否正确安装

```
 $ sudo docker run hello-world
```

稍等片刻，如果你看到 `Hello from Docker!` 字样，恭喜你，安装成功：-）