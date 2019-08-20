---
title: Spring Boot中使用Session
date: 2019-08-02 15:26:23
tags:
- Java
categories:
- java
- Spring Boot
---

## 基础知识

### <code>Cookie</code>

1. <code>cookie</code>由服务器端创建，然后添加到<code>HttpServletResponse</code>中发送给客户端（浏览器）。

2. 可以添加多个<code>cookie</code>键值对。

3. <code>cookie</code>由键值名和键值组成。相同<code>domain</code>和<code>path</code>中的键值名不能重复，添加键值名重名的键值对会覆盖上一个同名的键值对。
<!------------------------------------------more ------------------------------->
4. 添加<code>cookie</code>时，需要指定<code>cookie</code>所在域(<code>setDomain</code>), 路径(<code>setPath</code>)，指定存在时长(<code>setMaxAge</code>)。

5. 服务端创建好<code>cookie</code>后提交给客户端，之后浏览器的每次请求（<code>HttpServletRequest</code>）里都会携带cookie数组。

6. <code>springmvc</code>有两种方式获取：（1）在控制器函数中通过对参数注解<code>@CookieValue（键值名）</code>，获取指定某个<code>cookie</code>。（2）通过 <code>HttpServletRequest</code> 中的 <code>getcookies</code> 方法获取 <code>cookie</code> 数组，然后迭代里面的每一个 <code>cookie</code> 键值对。


### <code>Session</code>

1. 服务器会根据客户端的请求 <code>HttpServletRequest</code> 创建 <code>session=request.getSession()</code>。

2. 每一个 <code>session</code> 都有一个唯一的标示 <code>sessionID</code> ，可通过 <code>request.getSession().getId()</code>获得。

3. <code>session</code>是存储在服务器端的，每一个 <code>session</code> 都有一个id，当创建一个 <code>session</code> 后，会将该 <code>sessionID</code> 存放到此次访问的 <code>cookie</code> 中，当下次客户端的访问到来需要提取服务器中的 <code>session</code>时，会根据访问中 <code>cookie</code> 里的 <code>sessionID</code> 值来找到服务器中的具体 <code>session</code>。

4. 访问静态资源时不会创建 <code>session</code>

5. 服务器会把长时间没有活动的 <code>session</code> 从服务器内存中清除，此时 <code>session</code> 便失效。<code>Tomcat</code> 中 <code>session</code>的默认失效时间为 <code>20</code>分钟

6. <code>Tomcat 7</code>以上的版本中默认禁止客户端脚本读取<code>session Id</code>，需要在<code>context.xml</code>中设置<code>useHttpOnly="false"</code>，开启权限。

{% asset_img Headers.png 未开启读取权限时，响应中有<code>HttpOnly</code>标识 %}



## 在 <code>Spring Boot</code>中使用


### 设置<code>useHttpOnly</code>

- <code>jar</code> 项目

在配置文件中设置：

```properties
server.servlet.session.cookie.http-only=false
```

- <code>war</code>项目

在<code>context.xml</code>文件中的<code>Context</code>添加属性<code>useHttpOnly</code>

```xml
<Context useHttpOnly="false"><Context>
```

### 设置<code>Cookie</code>

场景: 首次请求时跳转到指定页面，页面再通过<code>ajax</code>向后台请求数据，进行页面渲染。跳转页面时，需要指定<code>cookie</code>，否则会出现项目部署到外部<code>tomcat</code>时，cookie无法写入的情况，造成<code>ajax</code>请求时，两次的<code>sessionId</code>不一致。
```java
@RequestMapping(value="/query", method=RequestMethod.GET)
public ModeAndView queryInfo(@RequestParam(value="sq")String sq,
                            HttpServletRequest request,
                            HttpServletResponse response
                            ) {
        
        ModeAndView view = new ModeAndView();
        //... view 操作
        Cookie[] cookies = request.getCookies();
        String sessionId = request.getSession().getId();
        if (cookies == null) {
            Cookie cookie = new Cookie("JSESSIONID", sessionId);
            cookie.setMaxAge(5 * 60);
            cookie.setDomian("localhost");
            cookie.setPath("/hand");
            response.addCookie(cookie);
        } else {
            for (Cookie cookie : cookies) {
                if (cookie.getName().equals("JSESSIONID")) {
                    cookie.setMaxAge(5 * 60);
                    cookie.setValue(sessionId);
                    cookie.setDomain("localhost");
                    cookie.setPath("/hand");
                    response.addCookie(cookie);
                    break;
                }
            }
        }

        return view;
}
```
### <code>ajax</code> 请求时带上<code>cookie</code>

在请求列表中加上<code>xhrFields</code>参数。

```js
var s = $.ajax({
    type: "get",
    timeout: 8000,
    xhrFields: { withCredentials: true},
    url: "/hand/val",
    data: {"params": ""},
    success: function(rel) {
        console.log(rel)
    },
    error: function() {
        console.log("failed")
    },
    complete: function(XMLHttpRequest, status) {
        if (status == "timeout") {
            s.abort();
            console.log("timeout");
        }
    }
})
```

## 参考：

<code>[1] Apache Tomcat 7 . The Context Container</code>  *http://tomcat.apache.org/tomcat-7.0-doc/config/context.html#Parallel_deployment*

<code>[2] springMVC操作cookie和session. Red_Code</code> *https://www.cnblogs.com/red-code/p/6629363.html*
