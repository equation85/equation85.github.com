---
layout: post
title: "R与大数据——bigmemory"
date: 2012-03-29 19:29
category: "技术"
tags: [R, Big Data, HPC]
---
{% include JB/setup %}

R中通过文件系统进行计算的几个包：
- **bigmemory**. 提供了创建、存储、访问、操纵超大矩阵的方法。可以将矩阵存储到共享内存（shared memory）或者内存映射文件（memory-mapped files）。
- **biganalytics**. 在bigmemory包的基础上进行扩展，提供了各种分析方法，如：kmeans、lm、glm以及一些基本统计量的算法。
- **bigtabulate**. 为big.matrix对象提供了table-、split-风格的方法。


#### 例子
	library(bigmemory)

	# 创建一个big.matrix对象。
	a<- big.matrix(nrow=10, ncol=3, type='integer', init=pi, dimnames=list(NULL,c('col1','col2','col3')), backingfile='bigdata.bin', descriptorfile='bigdata.desc')
	
	# 加载一个已有的big.matrix对象，可以实现不同R进程间的通信。
	b<- attach.big.matrix('bigdata.desc')

	# 修改big.matrix对象
	b[,1]<- sample(1:10)
	b[,2]<- sample(1:10)
	b[,3]<- sample(1:4,10,replace=T)
	flush(b)   # 更新backingfile，使改动生效

	# 按列排序
	a[morder(a,c(3,2)),]  # 和order类似
	mpermute(a,cols=1)  # 会修改backingfile

	# apply函数
	apply(a,1,mean)

	# kmeans
	x<- big.matrix(200,2,init=0,type='double')
	x[seq(1,by=2,length.out=100),]<- rnorm(200)
	x[seq(2,by=2,length.out=100),]<- rnorm(200,5,1)
	group<- bigkmeans(x,2)  # 需要foreach包

	# lm和glm
	x<- as.big.matrix(iris)
	lm.result<- biglm.big.matrix(Sepal.Length~Sepal.Width+Species, data=x, fc='Species')  # 需要biglm包

	# table函数
	bigtable(a,3)
	# 分组summary
	bigtsummary(a,ccols=3,cols=1)
	# split
	bigsplit(a,ccols=3,splitcol=NA)  # 返回每个分组的下标
	bigsplit(a,ccols=3,splitcol=2)  # 返回每个分组对应的第2列值
	sapply(bigsplit(a,3),length)  # 与sapply合用

