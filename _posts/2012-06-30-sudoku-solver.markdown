---
layout: post
title: "Sudoku"
date: 2012-06-30 12:27
category: "!正业"
tags: [R,tcl/tk]
---
{% include JB/setup %}

实在是懒的一个一个去解数独了。顺便接触了一个挺好玩的界面工具tk（[最有用的帮助](http://www.tcl.tk/man/tcl8.5/Keywords/contents.htm "tk help keywords")）。

界面如图：

![界面](/assets/images/20120630_sudoku1.png "Sudoku初始界面")![界面](/assets/images/20120630_sudoku2.png "Sudoku结果界面")

默认会有一组初值，如果需要自行输入则可在点击“Clear”之后输入，结果会用黑蓝两色区分。

首先计算每个空白处可能的值作为候选，然后基于下面三个规则进行计算：

1. 如果某行（列或大方块）内存在小方块，满足其候选值只有一个，那么该候选值就是小方块的值。  
2. 如果某行（列或大方块）内存在候选值，满足这个候选值在相应的行（列或大方块）内只出现一次，那么该候选值就是它所在的小方块的值。  
3. 如果当前没有满足规则1、2的情况，取候选值个数最少的小方块填入其中一个候选值，重复规则1、2。  


有兴趣的童鞋可以见[代码](https://github.com/equation85/ForFun/tree/master/sudoku "Sudoku代码")。
