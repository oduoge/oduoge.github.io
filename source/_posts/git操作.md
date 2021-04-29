---
title: Git日常操作
date: 2019-05-09 14:55:41
tags:
- Git
categories:
- 工具
toc: true
---


{% asset_img cobweb-4439844_960_720.jpg 风景 %}

记录一些在使用`git`过程中遇到的问题

<!----more ---->

1. 错误信息

- <code>ssh: connect to host github.com port 22: Connection timed out</code>

解决方式：
在<code>~/.ssh</code>目录下新建文件名为<code>config</code>的文件，加入以下信息

```sh
Host github.com
User git
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
Port 443
```
测试<code>ssh T git@github.com</code>


2. [推送本地分支](https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E8%BF%9C%E7%A8%8B%E5%88%86%E6%94%AF)

如果本地有一个名为<code>hexo</code>的分支需要放到远程仓库里。可以通过以下方式：

```sh
git push (远程仓库名) (分支名)
```

如：
```sh
git push origin hexo
# 上述命令是下面的简写，二者效果相同
git push origin hexo:hexo
```

- 获取远程分支

```sh
git fetch origin
git checkout -b serverfix origin/serverfix
```

- 同步远程仓库

```sh
git pull
```