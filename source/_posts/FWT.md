---
title: 浅谈位运算卷积
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2021-04-03 22:00:00
categories:
  - ACM
tags:
  - 位运算
  - 卷积
  - FWT
description: 俗称FWT
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/FWT/cover.jpg
---
# 一般形式

类比FFT,构造线性变换

于是设 $c(i,j)$ 为变换系数，即 $A[j]$ 对 $FWT(A)$ 的贡献系数。

$FWT(A)[i]=\sum\limits_{j=0}^{n-1}c(i,j)A_j$

可证 $c(i,j)c(i,k)=c(i,j⊕k)$ 证明略

另外,由于位运算每一位的独立性, $c(i,j)$ 也有一个重要性质: 可以分位考虑。

设二进制数 $a$ 的每一位分别为 : $a_0,a_1,a_2$ 

则有 $c(i,j)=c(i_0,j_0)c(i_1,j_1)c(i_2,j_2)...$ 就是把每一位的变换系数乘起来。

利用分治求解

设位矩阵为 $c=\begin{bmatrix}c(0,0)&c(0,1)\\c(1,0)&c(1,1)\end{bmatrix}$ 

对于某位运算构造出满足条件的可逆矩阵即可(理论上如此233)

# 任意位运算卷积

[题目链接](https://codeforces.com/gym/102956/problem/A)

[题解链接](https://disk.yandex.ru/i/MGHr-1zE2hBbKg)

题目给你每一位的位运算,求卷积

下面对题解进行简单的翻译,(假装我已经懂了

*20210405翻译初稿*

定义下标从 $0$ 开始.定义一个函数集合 $F_k$,函数接收两个 $[0,k)$ 的整数参数,返回值是 $[0,k)$ 的整数.
我们叫 $k \times k$ 的矩阵三元组 $(A,B,C)$ 是 $f \in F_k$ 的一个 *convolution triple* ,前提是: $C$ 是非退化矩阵(非异矩阵|满秩矩阵),并且该函数对于任意两个 $k$ 维向量 $x, y$ 执行以下过程

- 定义向量 $p=Ax,q=By$
- 定义向量 $r$ 是 $p,q$ 的点乘(内积)
- 返回 $z=C^{-1}r$

返回 $f$-convolution, i. e. $z_s = \sum\limits_{f(i, j)=s}{x_i y_i}$

现在对于给定的 bitwise-independent function(每位独立位运算) 我们想找到一个 *convolution triple* 使得卷得够快

对于给定函数 $f \in F_k$ 怎么检查这三个矩阵是否组成 *convolution triple* 呢?
必要的条件是：对于所有只有一个 1 其他都是 0 的向量,这个卷积应该是正确的
如果 $x$ 只有一个非零元素 $x_i=1$,$y$ 只有一个非零元素 $y_j=1$,那么$z$也应该只有一个非零元素$z_{f(i,j)}=1$.满足这个条件就够了因为 linearity of the convolution (卷积的线性?啥特征)

如果 $A={a_{ij}},B={b_{ij}},C={c_{ij}}$, 上述条件可以重写作: $C$ 必须是非退化矩阵,并且对于所有 $i,j,s\in {0,1,\dots,k-1}$,应满足 $a_{si}b_{sj}=c_{s,f(i,j)}$

现在我们可以尝试解决 $n=1$ 的情况,其中一种方法是对于任意二进制运算找到一个 *convolution triple*, 即枚举所有元素为 ${-1,0,1}$ 的矩阵三元组.事实证明对于所有 16 种运算我们都可以找到对应的矩阵.(注意:我们不能像做某些特殊的卷积那样假设 $A=B=C$, 因为对于函数 $f(x,y)=0$ 并没有满足的三元组)

假设有一个矩阵 $X={x_{ij}}$ of size $a$, $Y={y_{ij}}$ of size $b$. 把 $X ◦ Y$ 的结果表示为 $Z={z_{ij}}$ of size $ab$, $z_{bi+j,bk+l}=x_{ik}y_{jl},(0\leq i,k \leq a, 0\leq j, l \leq b)$ (换而言之,就是 $a^2$ 矩阵 $Y$ 放到一个矩阵 $X$ 并乘上来自 $X$ 对应的系数) (in other words, it’s just $a^2$ matrices $Y$ put into a matrix $X$ and multiplied by corresponding coefficients from $X$)

我们可以证明(通过检查上述条件)如果$(A_1, B_1, C_1)$ 是 $f\in F_a$ 的一个 *convolution triple*, $(A_2, B_2, C_2)$ 是 $g\in F_b$ 的一个 *convolution triple*, 那么 $(A_1 ◦ A_2, B_1 ◦ B)2, C_1 ◦ C_2)$ 是 $h\in F_{ab}$ 的一个 *convolution triple*, $h(bx_1+x_2,by_1+y2)=bf(x_1, y_1)+g(x_2,y_2)$.我们也可以证明如果 $C_1,C_2$ 都是非退化矩阵, 那么 $C_1◦ C_2$ 是非退化矩阵

因此我们只需要对于所有给定的函数 $f_i$ 求出 *convolution triples* $(A_i,B_i,C_i)$,那么 $(A,B,C)=(A_{n-1}◦\dots◦A_0,B_{n-1}◦\dots◦B_0,C_{n-1}◦\dots◦C_0)$ 就是一个原函数(运算)的 *convolution triple*

剩下的事情就是对 $A,B,C^{-1}$ 的快速乘法运算.因为所有这些矩阵都是 $2\times 2$ 的矩阵, $A(or B)$ 乘上向量 $v$ 的过程可以用雷同 $FFT$ 的做法,只有以下差别:当我们改变下标 $v[i]$ 和 $v[i+2^k]$,我们应该把矩阵 $A_k$ 运用到他们而不是蝴蝶变换 i. e.

$$\begin{pmatrix}
v_{new}[i]\\\\
v_{new}[i+2^k]
\end{pmatrix}
=A_k \cdot
\begin{pmatrix}
v_{old}[i]\\\\
v_{old}[i+2^k]
\end{pmatrix}$$

当把 $C^{-1}$ 乘上一个向量,我们可以类似上面过程反一下

总的时间复杂度是 $O(2^n \cdot n)$

另一种解题方法是只使用 $XOR$ and $AND/OR$ 的卷积,因为其他二进制函数既可以单独处理，也可以取反简化为这三个函数,也存在 $O(3^n)的解法但是不允许其通过此题$

# 参考资料
[2020-2021 Winter Petrozavodsk Camp, Belarusian SU Contest (XXI Open Cup, Grand Prix of Belarus)](https://codeforces.com/gym/102956)

[我的板子](https://kaizynx.github.io/2018/11/04/template/#%E5%BF%AB%E9%80%9F%E6%B2%83%E5%B0%94%E4%BB%80%E5%8F%98%E6%8D%A2-FWT)

[位运算卷积(FWT) & 集合幂级数](https://www.luogu.com.cn/blog/command-block/wei-yun-suan-juan-ji-yu-ji-kuo-zhan)

[洛谷P4717](https://www.luogu.com.cn/problem/P4717)

[K 进制 FWT / 高维多项式乘法 略记](https://www.luogu.com.cn/blog/user13052/fwt-algorithm)