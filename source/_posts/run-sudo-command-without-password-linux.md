---
title: 如何实现 ssh 无密码登录
date: 2018-04-12 00:18:38
categories: 技术
tags: Linux
copyright: false
---
<!--more-->
背景：你使用 `jeffer` 用户名登录服务器 `some.server.cn`
## 步骤：
1. 在 ~/.ssh 目录下 生成一对公私钥：

	`$ ssh-keygen -t rsa -f id_some_server`
	
2. 把公钥复制到服务器：

	 `$ ssh-copy-id  -i id_some_server.pub jeffer@some.server.cn`
	 
3. 测试效果：

	`ssh jeffer@some.server.cn`
	
4. 若有多对公私钥，可能需要在 `config` 文件里配置下别名：

	```
	Host some #别名
	HostName some.server.cn
	User jeffer
	PreferredAuthentications publickey
	IdentityFile ~/.ssh/id_some_server
	```
	
5. 可以使用别名快速登录：

	`ssh some`
	
## 常见问题
1. 登录时若仍然需要密码，可能需要在服务器上设置 ssh 目录的权限：

	`$ chmod  700  ~/.ssh/authorized_keys`

以上。
