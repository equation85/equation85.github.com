---
layout: post
title: "在博客中支持数学公式（二）"
date: 2014-02-26 21:05
category: "!正业"
tags: [JavaScript]
---

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({tex2jax: {inlineMath: [['$', '$'], ['\\(', '\\)']]}});
</script>
<script type="text/javascript"
  src="https://c328740.ssl.cf1.rackcdn.com/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

{% include JB/setup %}

两年前的一篇[文章](/blog/typing-equations-on-internet/)中写过可以用[ASCIIMathML](http://www1.chapman.edu/~jipsen/mathml/asciimath.html)来编写数学公式。然而该方法似乎在chrome下还是无法显示。

一次偶然发现了[MathJax](http://www.mathjax.org/)，功能相当强大。

#### 例如

当 $a \ne 0$ 时，方程 \\(ax^2 + bx + c = 0\\) 有两个解为 $$x = {-b \pm \sqrt{b^2 - 4ac} \over 2a}$$。

