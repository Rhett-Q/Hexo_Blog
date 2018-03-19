---
title: 探索VPN之PPTP
date: 2018-03-16 20:18:25
tags: vpn, CentOS
---
秉着对vpn(Virtual Private Network)的好奇，在阿里云搭建了一个基于PPTP协议的vpn，通俗的讲[vpn](https://zh.wikipedia.org/wiki/%E8%99%9B%E6%93%AC%E7%A7%81%E4%BA%BA%E7%B6%B2%E8%B7%AF)就是将两台在不同局域网中电脑联系在一个虚拟的局域网中，想着搭建完后在学校机房的电脑就可以和寝室的笔记本相互传输资料，不用带着个U盘来回跑。
## 前期准备
服务器是阿里云ESC的CentOS6.8，网络环境是校园网。由于学校的的路由器不支持PPTP穿透，一开始用电脑测试一直连不上，后来用手机试试居然连连上了。可能由于PPTP协议的不安全性，学习性质的搭建还是可以的，其他用途不建议。
## 安装必须的协议软件
`yum install -y pptpd ppp`  
## 配置主配置文件
	vim /etc/pptpd.conf
在文本末尾有系统推荐的ip网段，和本机的ip实际内网网段没联系，选择`localip`和`remoteip`将前面的`#`号删掉
	# (Recommended)
	#localip 192.168.0.1 建立连接后，分配给服务器的ip
	#remoteip 192.168.0.234-238,192.168.0.245 分配给客户端的ip
	# or
	#localip 192.168.0.234-238,192.168.0.245
	#remoteip 192.168.1.234-238,192.168.1.245`
## 配置账号文件
	vim /etc/ppp/chap-secrets
	# Secrets for authentication using CHAP
	# client    server  secret          IP addresses
	root * root *
	admin pptpd admin *
在文本末尾填写账号，服务的协议类型，密码，为客户端分配的ip段，`*`代表所有协议。
## 配置etc/ppp/options.pptpd
	vim etc/ppp/options.pptpd
修改DSN
	ms-dns 8.8.8.8
	ms-dns 8.8.8.8
如果你想打开debug模式，可以将下面的`#`号去掉
	#debug 调试模式
	#dump 服务启动时打印出所有配置信息
末尾加上
	logfile /var/log/pptpd.log 日志文件位置
