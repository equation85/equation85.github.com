---
layout: post
title: "Levenshtein Distance"
date: 2012-07-31 22:22
category: "修炼之路"
tags: [R, Algorithm]
---
{% include JB/setup %}

[Levenshtein distance](http://en.wikipedia.org/wiki/Levenshtein_distance)是一种常用的衡量两个字符串之间相似情况的度量。它是[编辑距离](http://en.wikipedia.org/wiki/Edit_distance)的最为广泛使用的一种定义，大多数情况下编辑距离被用来特指Levenshtein距离。所谓字符串A和B的编辑距离就是指一个字符串A需要经过多少次编辑可以得到字符串B。

要计算Levenshtein距离可以用动态规划的方法来进行。

#### 计算
假设我们需要计算两个字符串s和t的Levenshtein距离，其中s长为m，t长为n。记d\[i,j\]表示将子串s\[1...i\]转换为t\[1...j\]所需的最少操作数，那么Levenshtein距离即为d\[m,n\]。

有如下几个事实成立：

<ol>
<li>d[i,0]=i，d[0,j]=j。因为s[1..i]可以通过i次删除操作变为空字符串t[1...0]，所以d[i,0]=i。同样的d[0,j]=0。</li>
<li>如果s[i]=t[j]且d[i-1,j-1]=k，那么d[i,j]=k。因为最后一个字符相同，所以在将字符串s[1...i]变为t[1...j]时，只需考虑子串s[1...(i-1)]变为t[1...(j-1)]，由条件可知需k次编辑。</li>
<li>在其它情况下，距离为下面三种可能操作中编辑数最小的那个：
<ul>
	<li>插入。若d[i,j-1]=k，那么通过插入t[j]在k+1次编辑时可以得到t[1...j]。</li>
	<li>删除。若d[i-1,j]=k，那么通过删除s[i]在k+1次编辑时可以得到t[1...j]。</li>
	<li>替换。若d[i-1,j-1]=k，那么可以通过将s[i]替换为t[j]在k+1次编辑时得到t[1...j]。</li>
</ul>
</li>
</ol>

#### 代码
有兴趣的童鞋见[这里](https://github.com/equation85/codebase/blob/master/LevenshteinDistance/LevenshteinDistance.R)。

#### 可改进之处
<ul>
<li>可以减少空间复杂度到<em>O(min(m,n))</em>，而不是<em>O(m*n)</em>，因为在计算过程中每次仅需要前一行和当前行的数据。</li>
<li>可以将距离标准化到区间[0, 1]。</li>
<li>如果只关心距离是否小于某个给定的阈值<em>k</em>，那么只需要计算对角线上宽度为<em>2k+1</em>的方块。此时算法可以在<em>O(kI)</em>的时间内结束，其中<em>I</em>为短字符串的长度。</li>
<li>对于插入、删除、替换操作可以分别赋予不同的惩罚函数。同样可以根据插入、删除、替换的字符定义不同的惩罚函数。</li>
<li>将矩阵的首行置为0，则该算法可以用来在文本中进行模糊查找字符串。</li>
<li>由于计算过程前后的相关性，这个算法很难并行。However, all the <em>cost</em> values can be computed in parallel, and the algorithm can be adapted to perform the <em>minimum</em> function in phases to eliminate depedencies. （TODO: 暂时不是很理解它的做法，先记下来）</li>
<li>通过检查对角线而非行，并使用延迟计算（lazy evaluation）技术，可以在<em>O(m*(1+d))</em>的时间内（d为Levenshtein距离）完成距离的计算。当距离很小时，这种算法比常规的动态规划要快很多。</li>
</ul>

#### 上下限的性质

<ul>
<li>最小为两字符串长度之差。</li>
<li>最大为长字符串长度。</li>
<li>当且仅当两字符串相同时为0。</li>
<li>如果两字符串长度相同，其上限为Hamming距离。</li>
</ul>

### 延伸阅读：与其他编辑距离
Levenshtein距离并不是唯一常用的编辑距离，通过定义不同编辑操作可以得到不同的距离度量：

- 最长公共子序列（[longest common subsequence](http://en.wikipedia.org/wiki/Longest_common_subsequence_problem)）：只允许插入和删除，不能替换。
- [Demerau-Levenshtein距离](http://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance)：允许插入、删除、替换以及相邻字符间的交换。
- [Hamming距离](http://en.wikipedia.org/wiki/Hamming_distance)：只允许替换，因此也仅用于长度相同的字符串。


