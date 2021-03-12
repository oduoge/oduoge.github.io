---
title: JQuery使用
date: 2019-08-29 14:38:52
tags:
- JQuery
categories:
- 前端
- JavaScript
---

{% asset_img shanghai-3459422_960_720.jpg shanghai %}
## 兼容性

### 苹果手机 <code>jquery</code> 点击事件无效


页面在苹果移动设备上使用 <code>jquery click</code> 事件时，出现异常。

解决方法：

> 在点击当前元素上添加样式css的手势样式

```css
.next_button{        
    cursor: pointer    
}
```