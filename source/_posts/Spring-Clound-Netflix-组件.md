---
title: Spring Cloud Netflix 组件
date: 2019-07-26 16:33:34
tags:
- 文档摘要
categories:
- Spring Boot
- 后端
---

Spring Cloud Netflix [官方文档](https://cloud.spring.io/spring-cloud-netflix/spring-cloud-netflix.html)
- 组件

<code>Eureka</code>: 注册中心/服务发现
<code>Hystrix</code>: 断路器
<code>Netflix</code>: 路由网关
<code>Ribbon</code>: 负载均衡


## 服务发现

### <code>Eureka Clients</code>


### <code>Eureka Server</code>



## 断路器

### <code>Hystrix Clients</code>


### <code>Hystrix Dashboard</code>

## 负载均衡

### <code>Ribbon</code>

## 配置中心

### <code>Arhaius</code>

## 路由与网关

### <code>Zuul</code>

## 多程序语言支持

### <code>Sidecar</code>

## 请求的重试机制

<code>Spring Cloud Netflix</code>提供了多种方式用以发起 <code>HTTP</code>请求，可以使用<code>RestTemplate</code>、<code>Ribbon</code>、或者<code>Feign</code>. 不论使用何种方式，都有可能出现请求失败的情形。当一个请求失败后，通常希望其他能自动重新发起请求。因而可以通过添加<code>Spring Retry</code>的依赖，来处理这类需求。添加该依赖后，负载均衡的<code>RestTemplate</code>、<code>Feign</code>、<code>Zuul</code>会在请求失败的时候重新发起请求。

### BackOff Policies

>> 退避(backoff)是指当一个主机已经在有MAC 协议的网络中经历了一个冲突之后试图去重发之前的等待时期.

默认情况下，重试请求时没有使用退避机制的。如果要启动该功能。需要创建一个类型为<code>LoadBalancedRetryFactory</code>的<code>java bean</code>，并重写<code>createBackOffPolicy</code>函数，如：

```java
@Configuration
public class MyConfiguration {
    @Bean
    LoadBalancedRetryFactory retryFactory() {
        return new LoadBalancedRetryFactory() {
            @Override
            public BackOffPolicy createBackOffPolicy(String service) {
                return new ExponentialBackOffPolicy();
            }
        };
        
    }
}
```

### <code>Configuration</code>

#### <code>Zuul</code>