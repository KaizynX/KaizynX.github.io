---
title: hexo-mathjax 转义问题
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2020-04-23 19:30:00
mathjax: true
categories:
  - 技术
tags:
description: 解决 mathjax 公式无法换行问题
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/hexo_mathjax/cover.jpg 
---
之前遇到博客文章的公示显示错误

研究发现是换行没有起作用(换行是两个反斜杠`\`)

搜索发现这个是 hexo 的转义造成的

网上流传的方法都是改本地 `\node_modules` 里某个文件

可这也...首先这个文件夹存的是 nodejs 的一些插件等

其次我这可是高贵的自动部署博客,这种方法怎么适用呢

废话完 最终在知乎找到了答案

**使用四个反斜杠** `\\\\`

同理一个反斜杠不行的地方用两个反斜杠

[参考资料](https://www.zhihu.com/question/37542455/answer/120814953)