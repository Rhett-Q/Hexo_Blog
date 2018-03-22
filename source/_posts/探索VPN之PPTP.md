---
title: 探索VPN之PPTP
date: 2018-03-16 20:18:25
tags:
	- vpn
	- CentOS
---
秉着对vpn(Virtual Private Network)的好奇，在阿里云搭建了一个基于PPTP协议的vpn，通俗的讲[vpn](https://zh.wikipedia.org/wiki/%E8%99%9B%E6%93%AC%E7%A7%81%E4%BA%BA%E7%B6%B2%E8%B7%AF)就是将两台在不同局域网中电脑联系在一个虚拟的局域网中，想着搭建完后在学校机房的电脑就可以和寝室的笔记本相互传输资料，不用带着个U盘来回跑。
## 前期准备
服务器是阿里云ESC的CentOS6.8，网络环境是校园网。由于学校的的路由器不支持PPTP穿透，一开始用电脑测试一直连不上，后来用手机试试居然连连上了。
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

## 开启路由转发功能
	vim /etc/sysctl.conf
	net.ipv4.ip_forward = 1 开启路由转发功能
	sysctl -p 是配置立即生效

## 启动pptpd服务
	service pptpd start
	service pptpd stop
	service pptpd restart 重启时会有一个警告提示，在有vpn客户端连接的情况下，重启服务会造成，分配同一ip  
	给后来的客户端可以使用service pptpd restart-kill来关闭服务，再此开启pptp服务就没有警告。
### 做到这一步，如果你vpn没有访问外网的需求，就可以连接了。

## 开启iptables,修改防火墙规则
由于pptpd服务需要1723端口和gre协议

	iptables -A INPUT -p tco --dport 1723 -j ACCEPT
	iptables -A INPUT -p gre -j ACCEPT

现在开启防火墙也可以连接上vpn了，需要访问外网则需使用iptables的网络地址转换功能NAT  
将vpn内数据包的源地址修改为vps的内网地址
	iptables -t nat -A POSTROUTING -s 192.168.0.0/24(根据前面设置的vpn网段) -j SANT -to 172.18.122.157(我服务器的内网地址)
也可以使用MASQUERADE从网卡上自动以获取ip来替换数据包的源地址
	iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -o eth0 -j MASQUERADE
建议直接在配置文件中写入规则，否则重启iptables时会清除命令行写入的规则。

## 调试

到此为止，vpn算搭建好了，但是由于网络环境不同，会遇到不同的问题。我把自己遇到的问题写下来：  
	1.阿里云ESC的安全组规则没打开端口
	2.运行`lsmod|grep pptp`发现没有iptables模块，加上pptp模块

	modprobe ip_nat_pptp  
	modprobeip_conntrack_pptp

## 总结
基于pptp协议的VPN已经算是过时了，喜欢技术可以去研究，但是其他用途由于协议的不安全性不建议去搭建。

> 本文标题：探索VPN之PPTP  
> 发布时间：2018年3月16日  
> 更新时间：2018年3月17日
> 许可协议：[署名-非商用-相同方式共享4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)
