---
title: 脚本运行时出现Bad Interpreter
date: 2018-12-01 22:47:26
tags:
- 运维
categories:
- 操作系统
- Linux
---

脚本运行时出现Bad Interpreter

<!----more ----->

在 <code>windows</code> 下创建的 <code>shell</code> 文件上传到 <code>Linux</code> 服务器，运行时出现
```shell
/bin/sh^M: bad interpreter
```
可以用以下方式解决
<code>vi xxx.sh</code> (报错的那个文件)

```sh
:set fileformat=unix
:wq
```