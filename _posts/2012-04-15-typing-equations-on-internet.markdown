---
layout: post_eq
title: "在博客中支持数学公式"
date: 2012-04-15 22:10
category: "!正业"
tags: [JavaScript]
---
{% include JB/setup %}

要在网页中支持显示公式，最直接的做法就是通过LaTeX等工具将公式做成图片，然后在网页中导入就可以了。但如果真的用这种方法来写一篇数学类的文章，那显然非常的不方便。[这个网页](http://blog.sciencenet.cn/home.php?mod=space&uid=482644&do=blog&id=426697 "如何在网页中写数学公式")详细介绍了一些比较主流的书写数学公式的方法。[那个网页](http://hanamidesign.com/lab/math-with-html-css/prototype/ "html和css实现数学公式")通过纯CSS来实现公式，但支持还是比较有限，而且跟LaTeX语法差别还是比较大的。

对于建立在Github上的博客，还是比较推荐[ASCIIMathML](http://www1.chapman.edu/~jipsen/mathml/asciimath.html)。非常简单，只需在网页head中加上js代码就可以了。

	<script type="text/javascript" src="/path/to/ASCIIMathML.js"></script>

Normal distribution function: \`F(x;mu,sigma) = 1/(sqrt (2 pi) sigma) int_-oo^x e^(-(y-mu)^2/(2 sigma^2)) dy, -oo < x < oo\`

另外通过这个项目下的d.svg可以画出简单的图像，双击图片还能进行修改

Graph of \`sin(x)\` with "d.svg"

agraph plot(sin(x)) endagraph

本方法的缺点就是只支持IE6+(MathPlayer)、Firefox(MathML fonts)，在Chrome下无法正确显示公式，这点比较遗憾。
