---
title: 常用软件的配置文件在CentOS中的默认路径
date: 2019-07-08 17:25:01
tags:
categories:
- Linux
---

## <code>Apache</code>

首先执行命令找到 <code>httpd</code> 路径:
```
ps aux | grep httpd
```

 如 <code>httpd</code> 路径为 <code>/usr/local/apache/bin/httpd</code> 然后执行以下命令:

```
/usr/local/apache/bin/httpd -V | grep “SERVER_CONFIG_FILE”
```

即可找到编译时加载的配置文件路径 <code>httpd.conf</code>， <code>-V</code> 参数可以看到编译时配置的参数

## <code>Nginx</code>

首先执行命令找到 <code>Nginx</code> 路径
```
ps aux | grep nginx
```
如 <code>Nginx</code> 路径为
```
/usr/local/nginx/sbin/nginx
```
然后执行以下命令
```
/usr/local/nginx/sbin/nginx -V
```
默认放在安装目录下的 
```
conf/nginx.conf
```

## <code>Mysql</code>

首先执行命令找到 <code>Mysql</code> 路径
```
ps aux | grep mysqld
```
如mysqld路径为
```
/usr/bin/mysql
```
然后执行以下命令
```
/usr/bin/mysql –verbose –help | grep -A 1 ‘Default options’
```
或
```
/usr/bin/mysql –print-defaults
```

## <code>PHP</code>

### 1
可通过 <code>php</code> 函数 <code>phpinfo</code> 来查看，写个文件，然后用网址访问一下，查找 <code>“Loaded Configuration File”</code> 对应的值即为 <code>php</code> 加载的配置文件
### 2
如果是 <code>nginx + php</code> 配置，也可以通过查找 <code>php</code> 执行路径
```
ps aux | grep php
```
如，路径为 <code>/usr/local/nginx/sbin/php-fpm</code>
然后执行以下命令
```
/usr/local/nginx/sbin/php-fpm -i | grep “Loaded Configuration File”
```
即可看到 <code>php</code> 加载的配置文件
### 3
如果是 <code>apache + mod_php</code> 配置，也可以在 <code>apache</code> 配置文件中查看加载的 <code>php.ini</code> 路径。如 
```
PHPIniDir “/usr/local/apache/conf/php.ini”
```

当然也有简单的方法，就是通过find来搜索
如
```
find / -name nginx.conf
find / -name php.ini
find / -name my.cnf
find / -name httpd.conf
```
这种找法要经过刷选才行

[原文链接](http://www.111cn.net/sys/CentOS/63491.htm)