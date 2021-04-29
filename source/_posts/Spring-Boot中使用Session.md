---
title: Spring Boot中使用Session
date: 2019-08-02 15:26:23
tags:
- Spring Boot
categories:
- 后端
- Java
---

记录`Spring Boot`中操作`Session`的过程
<!---more ---->

## 基础知识  
### Cookie  
1. Cookie由服务器端创建，然后添加到 HttpServletResponse 中发送给客户端（浏览器）。  
2. 可以添加多个cookie键值对。  
3. Cookie 由键值名和键值组成。相同 domain 和 path 中的键值名不能重复，添加键值名重名的键值对会覆盖上一个同名的键值对。
<!------------------------------------------more ------------------------------->
4. 添加 Cookie 时，需要指定cookie所在域(setDomain), 路径(setPath)，指定存在时长(setMaxAge)。  
5. 服务端创建好cookie后提交给客户端，之后浏览器的每次请求（HttpServletRequest）里都会携带cookie数组。  
6. springmvc有两种方式获取：（1）在控制器函数中通过对参数注解@CookieValue（键值名），获取指定某个cookie。（2）通过 HttpServletRequest 中的 getcookies 方法获取 cookie 数组，然后迭代里面的每一个 cookie 键值对。

### Session  
1. 服务器会根据客户端的请求 HttpServletRequest 创建 session=request.getSession()。  
2. 每一个 session 都有一个唯一的标示 sessionID ，可通过 request.getSession().getId()获得。  
3. session是存储在服务器端的，每一个 session 都有一个id，当创建一个 session 后，会将该 sessionID 存放到此次访问的 cookie 中，当下次客户端的访问到来需要提取服务器中的 session时，会根据访问中 cookie 里的 sessionID 值来找到服务器中的具体 session。  
4. 访问静态资源时不会创建 session  
5. 服务器会把长时间没有活动的 session 从服务器内存中清除，此时 session 便失效。Tomcat 中 session的默认失效时间为 20分钟  
6. Tomcat 7以上的版本中默认禁止客户端脚本读取session Id，需要在context.xml中设置useHttpOnly="false"，开启权限。  
{% asset_img Headers.png 未开启读取权限时，响应中有HttpOnly标识 %}

## 在 Spring Boot中使用  
### 设置useHttpOnly  
- jar 项目  
在配置文件中设置：
```properties
server.servlet.session.cookie.http-only=false
```
- war项目  
在context.xml文件中的Context添加属性useHttpOnly
```xml
<Context useHttpOnly="false"><Context>
```
### 设置Cookie  
场景: 首次请求时跳转到指定页面，页面再通过ajax向后台请求数据，进行页面渲染。跳转页面时，需要指定cookie，否则会出现项目部署到外部tomcat时，cookie无法写入的情况，造成ajax请求时，两次的sessionId不一致。
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
### ajax 请求时带上cookie  
在请求列表中加上xhrFields参数。  
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
[1] Apache Tomcat 7 . The Context Container  *http://tomcat.apache.org/tomcat-7.0-doc/config/context.html#Parallel_deployment*  
[2] springMVC操作cookie和session. Red_Code *https://www.cnblogs.com/red-code/p/6629363.html*
