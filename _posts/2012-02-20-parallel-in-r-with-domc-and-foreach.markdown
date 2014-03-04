---
layout: post
title: "R与高性能计算——doMC+foreach实现并行"
date: 2012-02-20 16:20
category: "技术"
tags: [R, HPC]
---
{% include JB/setup %}

## 原理
**foreach**：支持*foreach*循环结构，即直接在一个对象的集合中进行迭代，而不需要指定具体的循环参数。foreach包主要考虑的是循环的返回值，与标准的lapply函数类似。同时foreach还支持并行计算。   
**doMC**：使用multicore提供并行计算的底层函数`%dopar%`。   
**multicore**：提供了一种在多核、多CPU的计算机上进行并行R计算的方法。所有的任务共享同样的初始工作空间，此外它还提供了收集各个核运算结果的方法。   

doMC包是foreach包的一种并行实现。它为foreach函数提供了一种并行处理的方式。doMC包可以理解为foreach和multicore两者之间的接口。   
在使用之前有以下几点需要注意：   

- multicore目前只能在支持fork调用的操作系统上，即Windows不可用。
- multicore只能在一台机器上进行，暂不支持集群。
- 不要从**GUI**环境运行doMC和multicore，因为GUI通常会调用计算机上所有的核。

为了使foreach以并行的方式执行，首先需要调用`registerDoMC`函数。该函数用来指定在执行任务时使用核的数量，在默认情况下，multicore包会使用`options('core')`中设定的核数。如果这个也没有设置，那么multicore则会尝试当前计算机上核的数量，然后使用所有它检测到的核。  
*注1：如果在foreach前没有调用registerDoMC，那么只会顺序计算。*  
*注2：registerDoSEQ将foreach计算模式换回顺序执行。*

## 例子
下面通过一个小例子来说明foreach的用法。采用Logistic回归来研究iris数据中花瓣长度和花种类之间的关系，通过bootstrap迭代10000次来求置信区间。

	library(doMC)
	registerDoMC()
	x<- iris[which(iris[,5]!='setosa'),c(1,5)]
	trials<- 10000
	# Parallel Computing
	ptime<- system.time({
		r<- foreach(icount(trials), .combine=cbind) %dopar% {
			ind<- sample(100,100,replace=T)
			result1<- glm(x[ind,2]~x[ind,1],family=binomial(logit))
			coefficients(result1)
		}
	})
	# Compared with Sequatial Computing
	stime<- system.time({
		r<- foreach(icount(trials), .combine=cbind) %dopar% {
			ind<- sample(100,100,replace=T)
			result1<- glm(x[ind,2]~x[ind,1],family=binomial(logit))
			coefficients(result1)
		}
	})

在我的主频2.1GHz双核Intel CPU上ptime为41秒，stime为70秒。很明显对于bootstrap这种“[Embarrassingly Parallel](http://en.wikipedia.org/wiki/Embarrassingly_parallel)”（求高手翻译这个名词）问题，只要简单的采用foreach模式就能带来很大的效率提升！
