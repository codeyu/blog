---
title: 在 CentOS/RHEL 7/6 和 Fedora 26/25 系统里如何安装 Java 8  
date: 2017-09-29 14:58:15  
categories: 翻译  
tags: [linux, java, how-to]  
copyright: false
---
<!--more-->
After a long wait, finally Java SE Development Kit 8 is available to download. JDK 8 has been released on Mar,18 2014 for general availability with the many featured enhancements. You can find all the enhancements in JDK 8 here.

![Java on Linux](http://codeyu.qiniudn.com/linux-with-java.png)

This article will help you to Install JAVA 8 (JDK/JRE 8u144) or update on your system. Read the instruction carefully before downloading Java from Linux command line. To Install Java 8 in Ubuntu and LinuxMint read [This Article](https://tecadmin.net/install-oracle-java-8-jdk-8-ubuntu-via-ppa/).

## Step 1 – Download Latest Java Archive

Download latest Java SE Development Kit 8 release from its official download page or use following commands to download from shell.
```sh
# cd /opt/
# wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u144-b01/090f390dda5b47b9b721c7dfaa008135/jdk-8u144-linux-x64.tar.gz"

# tar xzf jdk-8u144-linux-x64.tar.gz
```
## Step 2 – Install Java 8 with Alternatives

After extracting archive file use alternatives command to install it. alternatives command is available in chkconfig package.
```sh
# cd /opt/jdk1.8.0_144/
# alternatives --install /usr/bin/java java /opt/jdk1.8.0_144/bin/java 2
# alternatives --config java

There are 3 programs which provide 'java'.

  Selection    Command
-----------------------------------------------
*  1           /opt/jdk1.7.0_71/bin/java
 + 2           /opt/jdk1.8.0_45/bin/java
   3           /opt/jdk1.8.0_91/bin/java
   4           /opt/jdk1.8.0_144/bin/java

Enter to keep the current selection[+], or type selection number: 4
```
At this point JAVA 8 has been successfully installed on your system. We also recommend to setup javac and jar commands path using alternatives
```sh
# alternatives --install /usr/bin/jar jar /opt/jdk1.8.0_144/bin/jar 2
# alternatives --install /usr/bin/javac javac /opt/jdk1.8.0_144/bin/javac 2
# alternatives --set jar /opt/jdk1.8.0_144/bin/jar
# alternatives --set javac /opt/jdk1.8.0_144/bin/javac
```
## Step 3 – Check Installed Java Version

Check the installed Java version on your system using following command.
```sh
root@tecadmin ~# java -version

java version "1.8.0_144"
Java(TM) SE Runtime Environment (build 1.8.0_144-b01)
Java HotSpot(TM) 64-Bit Server VM (build 25.144-b01, mixed mode)
```
## Step 4 – Setup Java Environment Variables

Most of Java based application’s uses environment variables to work. Set the Java environment variables using following commands

* Setup JAVA_HOME Variable
```sh
# export JAVA_HOME=/opt/jdk1.8.0_144
```
* Setup JRE_HOME Variable
```sh
# export JRE_HOME=/opt/jdk1.8.0_144/jre
``` 
* Setup PATH Variable
```sh
# export PATH=$PATH:/opt/jdk1.8.0_144/bin:/opt/jdk1.8.0_144/jre/bin
```
Also put all above environment variables in /etc/environment file for auto loading on system boot.  
> 原文地址：https://tecadmin.net/install-java-8-on-centos-rhel-and-fedora/

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwMTEwODQyMjZdfQ==
-->