---
layout: post
title: "关于Mac OS X中R作图的字体"
date: 2012-08-13 21:32
category: Tips
tags: [R, Font]
---
{% include JB/setup %}

在Mac OS X中默认的图形设备是Quartz，不像Linux下的X11，如果不指定字体，在用R作图时默认是没办法显示中文的。

### 如何查看支持的字体
通过FontBook.app可以很容易的查看当前系统中可用的字体，info窗口中可以看到当前字体所支持的语言。通过搜索可以很方便的过滤出支持指定语言的字体，例如中文。

![Font Book](/assets/images/20120813_fontbook.png)

### 常规作图函数
对于plot等常规作图函数，至少有三种方法可以指定字体。

1. 在打开quartz设备的时候指定

	<pre>quartz(family='STKaiti')</pre>

2. 通过与设备无关的par指定

	<pre>par(family='STKaiti')</pre>

3. 在作图函数中指定参数family


### ggplot
ggplot作图时选择字体的机制似乎有所不同，在quartz和par中进行指定的方法都失效了。此时可以在相应的参数中通过`theme_text(family='STKaiti')`来设置所需字体。*有更好的方法欢迎留言*

### 代码及效果

	d <- data.frame(x=1:5,y=rnorm(5),label=c('一','二','三','四','五'))
	### For normal plot functions you could use
	# quartz(family='STKaiti')
	### Or
	# par(family='STKaiti')
	### Or
	plot(d[,1:2],main='中文',xlab='x轴',ylab='y轴',family='STKaiti')
	text(d[,1]+0.1,d[,2]-0.1,d[,3],family='STKaiti')
	### ggplot:
	ggplot(d,aes(x,y)) + geom_point() + geom_text(aes(x=x+0.1,y=y-0.1,label=label),family='STKaiti') + labs(x='x轴',y='y轴') + opts(title='中文',plot.title=theme_text(family='STKaiti'),axis.title.x=theme_text(family='STKaiti'),axis.title.y=theme_text(family='STKaiti'))

![plot font](/assets/images/20120813_plotfont.png) ![ggplot font](/assets/images/20120813_ggplotfont.png)

