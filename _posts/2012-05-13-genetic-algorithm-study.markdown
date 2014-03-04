---
layout: post_eq
title: "学习笔记——遗传算法"
date: 2012-05-13 17:19
category: "修炼之路"
tags: [Algorithm, R]
---
{% include JB/setup %}

在人工智能领域，遗传算法（GA, [Genetic Algorithm](http://en.wikipedia.org/wiki/Genetic_algorithm "Genetic Algorithm")）是一种模仿自然进化过程的启发式搜索算法，该方法通常被用在优化和搜索问题中。
遗传算法属于进化算法（EA, Evolutionary Algorithms）的范畴。
常用的方法有：继承（inheritance）、变异（mutation）、选择（selection）和交叉（crossover）。

[简单介绍](http://informatics.indiana.edu/larryy/al4ai/lectures/03.IntroToGAs.pdf)

[详细介绍](http://www.talkorigins.org/faqs/genalg/genalg.html)

典型的遗传算法中包括：

- 解空间的基因表达式（Genetic Representation）
- 适应度函数（Fitness Function)


### 遗传算法原型

	# Fitness: 适应度评分函数，为给定假设赋予一个评估函数
	# Fitness_threshold: 终止判定的阈值
	# p: 群体中包含的假设数量
	# r: 每一步中通过交叉取代群体成员的比例
	# m: 变异率
	GA(Fitness, Fitness_threshold, p, r, m) {
		初始化群体: P <- 随机产生p个假设
		评估: 对于P中的每一个假设h，计算Fitness(h)
		while( `max_(h)`(Fitness(h)) < Fitness_threshold ) {
			产生新一代`P_s`:
			1. 选择: 用概率方法选择P的(1-r)p个成员加入`P_s`。从P中选择假设`h_i`的概率为:
			`Pr(h_i) = (text{Fitness}(h_i)) / (sum_(j=1)^p text{Fitness}(h_j))`
			2. 交叉: 根据上面给出的`Pr(h_i)`，从P中选择`(r*p)/2`对假设。对于每对假设`<h_1,h_2>`，应用交叉算子产生两个后代。把所有后代加入`P_s`
			3. 变异: 使用均匀概率从`P_s`中选择`m%`的成员。对于选出的每个成员，在它的表示中随机选择一个位取反
			4. 更新: `P larr P_s`
			5. 评估: 对于P中的每个h计算Fitness(h)
		}
		从P中返回适应度最高的假设
	}

### 其他“选择”方法
在算法原型中所使用的选择方法通常被称为“适应度比例选择”（fitness proportionate selection）或者轮盘赌选择（roulette wheel selection）。另外也可以使用其他的方式来选择假设：

- 锦标赛选择（tournament selection）   
先从当前群体中随机选取两个假设，再以事先定义的概率p选择适应度较高的假设，以概率1-p选择适应度较低的假设。锦标赛选择常常比轮盘赌方法得到更*多样化*的群体。
- 排序选择（rank selection）   
当前群体中的假设先按适应度排序，某假设的概率与这个假设在排序列表中的位置成比例，而不是与它的适应度成比例。


### R Packages for GA

- [GALGO](http://biptemp.bham.ac.uk/vivo/galgo/AppNotesPaper.htm)：可以用来解决任何优化问题，特别适用于大数据集中的变量选择问题。
- [genalg](http://ftp.ctex.org/mirrors/CRAN/web/packages/genalg/index.html)：多维函数的优化问题。
- [mcga](http://ftp.ctex.org/mirrors/CRAN/web/packages/mcga/index.html)：处理实优化问题（real-valued optimization），使用比特而非实数的方式来表示变量，因此运算速度快。另外提供了`multi_mcga`函数来处理多目标优化问题。
- [rgenoud](http://ftp.ctex.org/mirrors/CRAN/web/packages/rgenoud/index.html)：结合遗传算法和基于微分（拟牛顿法，quasi-Newton）的方法来解决最优化问题。主要用来解决复杂函数的最大/最小问题，对于不可微的函数同样适用。

