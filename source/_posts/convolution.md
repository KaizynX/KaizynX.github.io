---
title: 卷积
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2020-04-01 23:00:00
mathjax: true
categories:
  - ACM
tags:
  - 卷积
  - FFT
description: 卷积学习笔记
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/convolution/cover.jpg 
---
# 卷积初步

## 一般线性卷积

$f[k]=\sum\limits_{i=0}^{k}a[i]\times b[k-i]$

a 的下标加上 b 的下标等于一个数

类比至多项式乘法 $x^i \times x^j = x^{i+j}$

使用 FFT 就可以快速求出 f 每一项

## 二般线性卷积

$f[k]=\sum\limits_{i=0}^{n-k}a[i]\times b[i+k]$

考虑一般卷积 $\sum\limits_{i=0}^{n-k}a[i]\times b[n-k-i] = f'[n-k]$

于是令 $f[k]=f'[n-k], b'[i] = b[n-i]$

此时 $f'[n-k]=\sum\limits_{i=0}^{n-k}a[i]\times b'[n-k-i]$

$=\sum\limits_{i=0}^{n-k}a[i]\times b[i+k] = f[k]$

### 例题

[hdoj6061 RXD and functions](http://acm.hdu.edu.cn/showproblem.php?pid=6061)

[参考博客](https://www.cnblogs.com/Yumesenya/p/7470832.html)

## 循环卷积

咕咕咕

# 参考资料

[浅谈FFT--从DFT到*CZT,及一些技巧](https://www.luogu.com.cn/blog/105254/qian-tan-fft-zong-ft-dao-fft)
