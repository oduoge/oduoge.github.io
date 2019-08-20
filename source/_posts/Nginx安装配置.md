---
title: Nginx安装配置
date: 2019-08-20 14:54:21
tags:
- nginx
categories:
- 后端
---

### 安装<code>nginx</code>开源版

通过<code>yum</code>源安装

1. 安装<code>EPEL</code>软件包

<code>yum install epel-release</code>

2. 更新<code>yum</code>源

<code>yum update</code>

3. 安装<code>nginx</code>开源版

<code>yum install nginx</code>

4. 验证是否安装成功

<code>nginx -v</code>

<!-------------more -------------->

### 启动<code>nginx</code>

#### 启动<code>nginx</code>

通过命令行启动，首先查找<code>nginx</code>的启动文件：
<code> whereis nginx</code>

通常是

<code>/usr/bin/nginx</code>

或者

<code>/usr/sbin/nginx</code>

在命令行中输入上述命令即可启动。

<code>nginx</code>可通过带参数的形式启动，如：

```
/usr/bin/nginx -t -c ~/mynginx.conf -g "pid /var/run/nginx.pid; worker_processes 2;"
```

可选项：

|参数| 含义|
|:------|:------|
|<code>-?, -h</code>| 帮助信息|
|<code>-v</code>| 版本|
|<code>-V</code>| 版本, 编辑器的版本信息, 当前配置信息|
|<code>-t</code>| 测试配置文件，服务实际上不会启动。主要检查配置文件中可能存在的格式和符号问题|
|<code>-q</code>| 不输出在测试阶段的非错误信息|
|<code>-s signal</code>| 向主进程发送指令信号：<code>stop, quit, reopen, reload (version >= 0.7.53)</code>|
|<code>-p prefix</code>| 设置前缀路径，默认值为<code>/usr/local/nginx</code>需要<code> (version >= 0.7.53)</code>|
|<code>-c filename</code>| 手动指定生效的配置文件|
|<code>-g directives</code>| 设置全局指令|

> <code>nginx</code>的可用参数比较少，几乎所有配置都是通过配置文件运行的

#### 停止或重启<code>nginx</code>服务

对于已在运行的<code>NGINX</code>服务，有两种方式去控制它：
1. 通过指令

```
/usr/bin/nginx -s stop
```

2. 通过向<code>NGINX</code>主进程发送信号

如优雅的关闭<code>NGINX</code>服务：

```
kill -QUIT $( cat /usr/local/nginx/logs/nginx.pid )
```

<code>NGINX</code>的主进程可以处理以下指令：

| 指令 | 含义|
|:------|:-------|
|TERM, INT|	Quick shutdown|
|QUIT|	Graceful shutdown|
|KILL|	Halts a stubborn process|
|HUP|	 Configuration reload <br />Start the new worker processes with a new configuration<br />Gracefully shutdown the old worker processes|
|USR1	|Reopen the log files|
|USR2	|Upgrade Executable on the fly|
|WINCH	|Gracefully shutdown the worker processes|

没有必要处理其他的工作进程，但它们也支持下列指令：

| 指令 | 含义|
|:------|:-------|
|TERM, INT|	Quick shutdown|
|QUIT|	Graceful shutdown|
|USR1	|Reopen the log files|

另外<code>NGINX</code>支持在运行阶段更新配置信息<sup>[1]</sup>



[1] <code>Starting, Stopping, and Restarting NGINX</code> (_https://www.nginx.com/resources/wiki/start/topics/tutorials/commandline/_)