---
title: Service Mesh简介
date: 2019-07-31 09:55:27
tags:
- Service Mesh
categories:
- 后端
- 框架
---

> Service Mesh 是一个基础设施层，用于处理服务间通信。云原生应用有着复杂的服务拓扑，Service Mesh 可以保证 Request 可以在这些拓扑中可靠地穿梭。在实际应用当中，Service Mesh 通常是由一系列轻量级的网络代理组成的，它们与应用程序部署在一起，但对应用程序透明。

`Service Mesh`具有如下优点：
- 屏蔽分布式系统通信的复杂性（负载均衡、服务发现、认证授权、监控追踪、流量控制等）服务只用关注业务逻辑。
- 真正的语言无关，服务可以用任何语言编写，只需和`Service Mesh`通信即可。
- 对应用透明，`Service Mesh`组件可以单独升级。

`Service Mesh`所面临的挑战：
- `Service Mesh`组件以代理模式计算并转发请求，一定程度上会降低通信系统性能，并增加系统资源开销。
- `Service Mesh`组件接管了网络流量，因此服务的整体稳定性依赖于`Service Mesh`，同时，额外引入的大量`Service Mesh`服务实例的运维和管理也是一个挑战。