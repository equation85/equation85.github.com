---
layout: post_eq
title: "对数次递归计算Fibonacci数列"
date: 2012-05-10 19:17
category: "!正业"
tags: [Algorithm, R]
---
{% include JB/setup %}

翻了翻博客，距上一次更新差不多快一个月了。在摸黑起床又摸黑睡觉的忙了半个月之后，又迷上了两个APP（[Hunters 2](http://itunes.apple.com/us/app/hunters-2/id489463556?mt=8 "Hunters 2")、[Mini Motor Racing](http://itunes.apple.com/us/app/mini-motor-racing/id426860241?mt=8&ign-mpt=uo%3D2 "Mini Motor Racing")），也就懒得更新了。

今天在朋友的博客上看到一篇文章[对数步计算斐波那契数列](http://blog.csdn.net/largetalk/article/details/7552855 "对数步计算斐波那契数列")（不知为什么当时首先想到的是当年奎哥的口头禅“小技巧”，看来“中毒”颇深，呵呵！），觉得挺有意思的便来证明一下。

根据Fibonacci数列的定义，很直观的可以通过重复下面这个过程来进行计算

\`{(a larr a + b),(b larr a):}\`

这个过程很容易转化为矩阵形式：

\`((a),(b)) larr ((1,1),(1,0))((a),(b))\`

更一般的，考虑线性变换

\`f_(p,q) = ((p+q,q),(q,p))\`

显然，Fibonacci数列的每一次迭代可表示为\`f_(0,1)\`。下面考虑一般化的情形，计算第n个Fibonacci数的过程可表示为

$f^n := (f_{p,q})^n = \overbrace{f \cdot f \cdots f}^{n个}$

通过矩阵运算可知

\`f^2 = ((p+q,q),(q,p))((p+q,q),(q,p)) = ((p^2+2pq+2q^2,2pq+q^2),(2pq+q^2,p^2+q^2)) = ((p'+q',q'),(q',p')) := f_(p',q')\`

其中\`{(p' = p^2 + q^2),(q' = 2pq + q^2):}\`

所以\`f^n\`可分解为

\`f^n = {(f * f^(n-1), text{if n is odd}), ((f^2)^(n/2) = f_(p',q')^(n/2), text{if n is even}):}\`

于是也就有了下面这样的递归实现（这里我再转一下，不过是R的）

	fibonacci <- function(n, a, b, p, q) {
	  if(n==0)
	    return(b)
	  if(n%%2==0)
	    return(fibonacci(n/2,a,b,p*p+q*q,2*p*q+q*q))
	  return(fibonacci(n-1,a*p+a*q+b*q,a*q+b*p,p,q))
	}
	# 计算前10个数
	mapply(fibonacci,n=1:10,MoreArgs=list(a=1,b=0,p=0,q=1))
	 [1]  1  1  2  3  5  8 13 21 34 55
