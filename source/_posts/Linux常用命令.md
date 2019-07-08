---
title: Linux常用命令
date: 2019-07-08 16:04:01
tags:
categories:
- linux
---

## 开放端口
### <code>CentOS 7</code>

查看防火墙状态: 
```
firewall-cmd --state
```
开启 <code>80</code> 端口:
```
firewall-cmd --zone=public --add-port=80/tcp --permanent
```
重启防火墙: 
```
firewall-cmd --reload
```
查看端口开启状况: 
```
iptables -L -n 
```