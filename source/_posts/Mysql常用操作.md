---
title: Mysql常用操作
date: 2019-08-19 14:44:16
tags:
- tips
categories:
- Mysql
---

### 重启服务

- CentOS7

<code>systemctl restart mysqld</code>

### 查询设置

查看 <code>max_allowed_packet</code> 大小: <code>show VARIABLES like '%max_allowed_packet%';</code>


### 日志相关

```
mysql>set global log_output=file;//输出到文件
mysql>set global general_log_file='file-path.log';//输出文件地址
mysql>set global general_log=on;//开启通用日志
mysql>set global general_log=off;//关闭通用日志


mysql有以下几种日志：  
   错误日志：     -log-err  
   查询日志：     -log  
   慢查询日志:   -log-slow-queries  
   更新日志:     -log-update  
   二进制日志： -log-bin  


是否启用了日志 
mysql>show variables like 'log_%'; 

怎样知道当前的日志 
mysql> show master status; 

```