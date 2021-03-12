---
title: CSS色彩控制
date: 2019-07-22 16:33:45
tags:
- CSS
categories:
- 前端
- CSS
---

## [颜色](https://www.cnblogs.com/xuan-0107/p/4448068.html)

### RGBA


RGB是一种色彩标准，是由红(R)、绿(G)、蓝(B)的变化以及相互叠加来得到各式各样的颜色。RGBA是在RGB的基础上增加了控制alpha透明度的参数。
<!--------more -------->
语法：
```
color：rgba(R,G,B,A)
```
R、G、B三个颜色参数，正整数值的取值范围为：0 - 255。
百分数值的取值范围为：0.0% - 100.0%。超出范围的数值将被截至其最接近的取值极限。并非所有浏览器都支持使用百分数值。

A为透明度参数，取值在0~1之间，不可为负值。

代码示例：
```
background-color:rgba(100,120,60,0.5);
```

### 渐变色彩

CSS3 Gradient 分为线性渐变(linear)和径向渐变(radial)

```
background-image:linear-gradient(to left, red, orange,yellow,green,blue,indigo,violet);
```

<code>linear-gradient(to bottom, ...)</code>第一个参数表示角度，后面的参数表颜色的起始点和结束点，可以有多个值。
第一个参数：

|角度|英文|作用|
|:---------:|:-----------:|:-------------:|
|0deg| to top|从下向上|
|90deg| to right| 从左向右|
|180deg| to bottom|从上向下|
|270deg| to left|从右向左|