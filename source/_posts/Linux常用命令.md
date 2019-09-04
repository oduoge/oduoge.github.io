---
title: Linux常用命令
date: 2019-07-08 16:04:01
tags:
- tips
categories:
- Linux
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

## NPT时钟同步

手动同步: <code>ntpdate -u server-ip</code>
编辑<code>ntp</code>配置文件：<code>vim /etc/ntp.conf</code>:
```
server server-ip
restrict server-ip nomodify notrap noquery

server 127.127.1.0
fudge 127.127.1.0 stratum 10
```

启动<code>ntp</code>服务：<code>service ntpd start</code>
查看同步情况：<code>ntpq -p</code>


## 解压/压缩命令