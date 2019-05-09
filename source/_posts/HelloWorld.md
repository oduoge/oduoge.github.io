---
title: HelloWorld
date: 2018-04-14 11:59:02
tags:
- 测试
- hexo
categories: 搭建博客
---

#### 基本操作

- 新建分类
```
hexo new page handwriting
## 在新生成的handwwriting目录下的index.md中添加type项.
---
title: handwriting
date: 2018-12-01 23:10:11
type: handwriting
---
```
- 允许文章添加图片

修改<code>_config.yml</code>配置文件中的<code>post_asset_folder</code>项为<code>true</code>

创建新文章
```
hexo new "新文章名"
```
之后在<code>source/_posts</code>下会生成一个名为"新文章名"的文件夹和一个名为"新文章名"的<code>.md</code>文件，文件夹中放置需要收入的图片，在<code>.md</code>文件中引入图片的方式为
```
{% asset_img 需要引入的图片名.png 图片说明 %}
```
- 编译文件
```
hexo generate
hexo g
```
- 提交

提交之前可用<code>hexo clean</code>删除以前的内容，也可以不用
```
hexo d
hexo deploy
```
