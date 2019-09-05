---
title: Mysql最大连接数
date: 2019-08-19 15:01:04
tags:
- tips
categories:
- Mysql
---

{% asset_img flowers-4436910_960_720.jpg flowers %}
Mysql中可以通过设置<code>max_connections</code>参数来控制数据库的最大连接数:
<!---------------more --------------------->
- <code>max_connections</code>

|Property|Value|
|:-------|:-------|
|Command-Line Format| --max-connections=#|
|System Variable|max_connections|
|Scope|Global|
|Dynamic|Yes|
|Tyep|Integer|
|Default Value|151|
|Minimum Value| 1|
|Maximum Value| 100,000|

最大连接数，指的是客户端可以在同一时刻连接数据库服务的最大数目。


### MySQL是如何控制客户端连接的

#### 网络接口和管理连接的线程

服务端能够通过多种网络接口监听客户端连接。通过这些接口，管理连接的线程可有效的处理客户端的连接请求，服务端主要监听以下内容：
- 在所有的平台上，都有一个<code>管理线程</code>来处理 <code>TCP/IP</code>请求
- 在<code>Unix</code>上，该<code>管理线程</code>同时也会处理<code>Unix</code>的<code>socket</code>请求
- 在<code>Windows</code>服务器上一个<code>管理线程</code>会处理在<code>共享内存</code>中的连接请求，另一个线程处理<code>命名管道连接请求</code>

服务端不会创建线程去处理那些没被监听的网络接口。例如，假如一台<code>Windows</code>服务器支持<code>命名管道</code>的服务没有开启，那么<code>MySQL</code>服务端也不会创建一个新的线程去处理它们。

#### 客户端连接线程的管理

每一个到达服务端的客户端连接，服务端都会为其提供一个线程，这个线程会处理与之相对应的客户端连接的认证和请求。
线程管理器会首先查看缓存中是否有可用线程用于提供给客户端，如果没有的话，则新建一个线程。
当一个连接算是完成时，它所对应的服务线程如果不空，则会被放入到缓存中。


在该连接模型下，会存在当前有多少个连接请求，就会有多少个线程的情况，服务端就会被迫处理大量的请求，这会造成多方面的不利因素。比如，线程的创建和销毁的开销会增大，同时，每个线程都需要服务端和内核的资源，如栈空间。为了调度大量的并发连接，每个线程的栈空间必须保持在一个较低的值，这会导致出现栈空间太小或者服务端消耗大量的内存。也可能出现其他资源的耗尽，造成调度开销变得非常大。

<code>MySQL Enterprise Edition</code> 包含一个线程池的插件，提供了一个可选的线程处理模型，能够减少开销提高性能。




[1] MySQL 5.7 Reference Manual _https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html_

