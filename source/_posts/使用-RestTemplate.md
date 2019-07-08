---
title: 使用 RestTemplate
date: 2019-07-08 17:01:14
tags:
categories:
- Spring Boot
- java
---

## <code>Not enough variables available to expand</code>

现有请求链接:
```
http://example.com?criteria={startDate:["2018-12-05"], endDate:["2018-12-07"]}
```

- 准备工作

```java
class Criteria {
    List<String> startDate;
    List<String> endDate;
}
```

```java
// 假设现已有bean criteria
// 转换成 json字符串
// <see>com.fasterxml.jackson.databind.ObjectMapper</see>
ObjectMapper mapper = new ObjectMapper();
String json = mapper.writeValueAsString(criteria);
```
此时如直接用 <code>getForObject(String url, Class<T> responseType, Object... uriVariables)</code> 进行请求 :

```java
String url = "http://example.com?criteria=" + json;
Object obj = restTemplate.getForObject(url, Object.class);
```

会报 <code>Not enough variables available to expand</code> 的错误。
这是因为 <code>getForObject(String url, Class<T> responseType, Object... uriVariables)</code> 会对 <code>url</code> 进行转换，会把 <code>{}</code> 当成了占位符，为了避免这种情况，可以直接使用 <code>T getForObject(URI url, Class<T> responseType)</code> 方法，自己对链接进行转换:

```java
// java.net.URI
String url = "http://example.com" + "?criteria={json}";
// build URI
UriComponents uriComponents = UriComponentsBuilder.fromUriString(url)
        .build().expand(json) // 指定参数，多个参数按出现的前后顺序添加 .build(json1, json2)
        .encode();
URI uri = uiComponents.toUri();
Object result = restTemplate.getForObject(uri, Object.class);
```

## <code>GET</code> 方式发送数据，服务器端拿到的数据为空

请求参数需要以占位符的形式才能获取值：
```java
Map<String, String> map = new HashMap<>();
map.put("p1", "myValue");
String url = "http://localhost:8080/sayHello";
String paramedUrl = "http://localhost:8080/sayHello?p1={p1}";
System.out.println(restTemplate.getForObject(paramedUrl, String.class, map));
```

