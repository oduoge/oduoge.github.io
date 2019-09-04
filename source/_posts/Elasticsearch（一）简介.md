---
title: Elasticsearch（一）简介
date: 2019-07-26 10:14:01
tags:
- Elasticsearch
categories:
- 后端
---

搜索引擎三大过程，爬取内容、进行分词、建立反向索引。

存储海量数据的分布式搜索引擎
特有概念
索引、类型、文档
（数据库、表、行）

索引 - 存放数据
类型=定义数据结构
文档-数据


分布式存储
会对数据进行切分，同时每一个分片会保存多个副本，和HDFS一样，是为了保证分布式环境的高可用。
master - slave
节点是对等的，节点间会通过自己的一些规则选取集群的Master，Master会负责集群状态信息的改变，并同步给其他节点。

请求建立索引 -> master -> slave1/2...（信息同步）
只有建立索引和类型需要经过master，数据的写入有一个简单的routing规则，可以route到集群中的任意节点，所以数据写入压力是分散在整个集群的。

应用场景
ELK 系统，也就是日志分析系统，其中E是指Elasticsearch，L 是Logstash,是一上日志收集系统，K是Kibana，是一个数据可视化平台。

logstash采集业务系统日志，存储到es中，通过kibana展现给运维人员分析。
日志接入elk系统，可以在系统运行过程中，及时定位异常，和实时报警


## 安装

### 7.x
在 <code>/etc/yum.repos.d/</code> 目录下新建文件 <code>elasticsearch.repo</code> ，加入以下内容:

```
[elasticsearch-7.x]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

保存后。执行安装命令：

```sh
sudo yum install elasticsearch
```

## 启动

安装完成后，<code>Elasticsearch</code> 还没有启动。查看当前服务管理工具：

```
ps -p 1
```

### 以<code>systemd</code>启动

- 随系统启动

```
sudo /bin/systemctl daemo-reload
sudo /bin/systemctl enable elasticsearch.service
```

- 启动、停止

```
sudo systemctl start elasticsearch.service
sudo systemctl stop elasticsearch.service
```


- 6.x