---
layout: post
title: "R与大数据——Map/Reduce"
date: 2012-03-31 23:27
category: "技术"
tags: [R, Big Data, HPC]
---
{% include JB/setup %}

在R+Hadoop的模式下工作了也有一个月了，小结一下这段时间以来的心得体会以加深理解。  
作为当下最火的大数据解决方案，Hadoop提供了两种方式来实现Map/Reduce：
- [Hadoop Streaming](http://hadoop.apache.org/core/docs/r0.18.2/api/org/apache/hadoop/streaming/package-summary.html "Hadoop Streaming")，允许用户创建和运行可执行程序来作为mapper和reducer。本文主要考虑这种方式。 
- [Hadoop Pipes](http://hadoop.apache.org/core/docs/r0.18.2/api/org/apache/hadoop/mapred/pipes/package-summary.html "Hadoop Pipes")，是一个C++ API，也可用于实现Map/Reduce应用。由于它主要面对C++用户，所以不在本文考虑范围之内。

### Hadoop Streaming的工作原理
在Hadoop Streaming中，mapper和reducer都是可执行文件，它们从标准输入stdin中读取数据，经过处理再输出到标准输出stdout。Hadoop Streaming会自动创建map/reduce作业（job）并将其提交到合适的集群中，监控任务进程直到完成。

当指定好mapper的可执行文件时，在初始化的过程中每个mapper任务（task）会为该可执行文件单独创建一个进程。在mapper任务运行时，它将输入转化为行并发送到进程的stdin。同时mapper会收集从进程的stdout产生的输出，并将得到的每一行转化为key/value对作为mapper的输出。默认情况下，每行第一个`\t`字符之前的内容作为key，余下的所有内容（除了`\t`字符）为value。

当指定好reducer的可执行文件时，在初始化的过程中每个reducer任务（task）也会为该可执行文件单独创建一个进程。在reducer任务运行时，它将输入的key/value对转化为行并发送到进程的stdin。同时收集从进程的stdout产生的输出作为reducer的输出。

### R+Hadoop=?
从Hadoop Streaming的工作原理中可以看到只要具有“能够从stdin读入数据，输出数据到stdout”的能力，就可以利用它来执行Map/Reduce任务。显然R也能够在Map/Reduce的框架下发挥它统计和数据挖掘的优势了。

目前在CRAN上比较成熟的和Hadoop有关的包有：
- **[HadoopStreaming](http://cran.csdb.cn/web/packages/HadoopStreaming/index.html "HadoopStreaming")** 提供了一个编写R Map/Reduce脚本的框架。好处在于它不但能够在集群环境下工作，而且在非集群环境下通过管道也能进行Map/Reduce。这就为算法的调试带来了方便。
- **[Rhipe](http://www.rhipe.org "Rhipe")** 没有用过，具体不好多说。但是它提供了一个类似lapply的函数rhlapply，这点看起来比较方便。
- **[RHive](http://cran.csdb.cn/web/packages/RHive/index.html "RHive")** 提供了R和Hive之间的交互，在它的基础上可以用R实现UDF（User Defined Function）和UDAF（User Defined Aggregate Function）以便在Hive中调用（[例子](http://www.slideshare.net/SeonghakHong/r-hive-tutorial-udf-udaf-udtf-functions "RHive UDF")）。这在某些应用场景是非常有用的。
- **[RHadoop](https://github.com/RevolutionAnalytics/RHadoop "RHadoop")** 由三个包rhbase, rhdfs, rmr共同组成，属于[Revolution Analytics](www.revolutionanalytics.com "Revolution Analytics")的项目。实际上这个包的作者就是Rhipe的作者，是他在进入RA之后负责开发的。有了它，Rhipe就是浮云了。

### Word Count Example
这里给出一个Map/Reduce中的“Hello World”小例子 —— Word Count。  
采用HadoopStreaming包进行实现，改自其自带例子，进行简化，也更实用。

	-------------- mapper.R --------------
	#!/usr/bin/env Rscript
	library(HadoopStreaming)
	options(stringsAsFactors=F)
	opts <- hsCmdLineArgs()
	map <- function(inflow) {
		inflow <- strsplit(paste(inflow,collapse=' '),'[[:punct:][:space:]]+')[[1]]
		inflow <- inflow[!(inflow=='')]
		hsWriteTable(data.frame(word=inflow,cnt=1),
	                 file=opts$outcon,sep=opts$outsep)
	}
	hsLineReader(opts$incon,chunkSize=opts$chunksize,FUN=map)
	
	-------------- reducer.R --------------
	#!/usr/bin/env Rscript
	library(HadoopStreaming)
	options(stringsAsFactors=F)
	opts <- hsCmdLineArgs()
	reduce <- function(inflow) {
		out <- tapply(inflow[,2],inflow[,1],sum)
		hsWriteTable(data.frame(word=names(out),cnt=unname(out)),
		             file=opts$outcon,sep=opts$outsep)
	}
	p.key <- NA
	p.val <- 0
	p.reduce <- function(inflow) {
		if(nrow(inflow)==0) {
			cat(paste(p.key,opts$outsep,p.val,'\n',sep=''),
			    file=opts$outcon)
			p.key <<- NA
			p.val <<- 0
		} else {
			if(is.na(p.key)) {
				p.key <<- inflow[1,1]
			}
			p.val <<- p.val+sum(inflow[,2])
		}
	}
	cols <- list(word='',cnt=0)
	hsTableReader(opts$incon,cols,chunkSize=opts$chunksize,skip=opts$skip,sep=opts$insep,keyCol='word',singleKey=F,ignoreKey=F,FUN=reduce,PFUN=p.reduce)

### 结语
通过Hadoop Streaming机制可以发挥Hadoop的集群能力和R的统计挖掘能力。虽然[它并不是那么的elegant](http://www.zdnet.com/blog/big-data/mapreduce-streaming-beyond-java/264 "Streaming beyond Java")，但是确实提供了一种在大数据的情况下解决问题的方式。不过使用HadoopStreaming包来计算的话，自行进行一定的封装还是必要的，否则每次都要写一大堆脚本才能完成任务。毕竟Done in R style还是很重要滴！
