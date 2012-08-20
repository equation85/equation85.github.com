---
layout: post
title: "Markdown语法示例"
date: 2012-02-16 20:34
category: "技术"
tags: [Other]
---
{% include JB/setup %}

[上一篇文章](http://equation85.github.com/blog/blog-with-github-and-jekyll)中介绍了如何通过Github和Jekyll搭建个人博客。而要想方便的进行博客写作，那么Markdown是必不可少的工具。本文将通过一些简单的例子来说明Markdown的用法。

- - - 

## 什么是Markdown
Markdown是一个将文本转化为HTML的工具。简单来说，Markdown是一个兼顾可读性与易用性的轻量级标记体系。Markdown并不追求大而全，它只关心HTML里最常用的几个标记，对于一些不常用的标记它允许直接将HTML标记插入文本。

- - - 

## 标题
Markdown提供了两种方式（Setext和Atx）来显示标题。
#### 语法：

	Setext方式
	标题1
	=================

	标题2
	-----------------

	Atx方式
	# 标题1
	## 标题2
	###### 标题6

#### 效果：   
Setext方式   
标题1
=================

标题2
-----------------

Atx方式   
# 标题1
## 标题2
###### 标题6

- - - 

## 换行
在文字的末尾使用两个或两个以上的空格来表示换行。

- - - 

## 引用
行首使用`>`加上一个空格表示引用段落，内部可以嵌套多个引用。   
#### 语法：

	> 这是一个引用，
	> 这里木有换行，   
	> 在这里换行了。
	> > 内部嵌套

#### 效果：
> 这是一个引用，
> 这里木有换行，   
> 在这里换行了。
> > 内部嵌套

- - - 

## 列表
__无序列表__使用`*`、`+`或`-`后面加上空格来表示。  
#### 语法：

	* Item 1
	* Item 2
	* Item 3

	+ Item 1
	+ Item 2
	+ Item 3
	
	- Item 1
	- Item 2
	- Item 3

#### 效果：

* Item 1
* Item 2
* Item 3

+ Item 1
+ Item 2
+ Item 3

- Item 1  
- Item 2
- Item 3

__有序列表__使用数字加英文句号加空格表示。   
#### 语法：

	1. Item 1
	2. Item 2
	3. Item 3

#### 效果：

1. Item 1
2. Item 2
3. Item 3

- - - 

## 代码区域
__行内代码__使用**反斜杠**<code>`</code>表示。   
__代码段落__则是在每行文字前加4个空格或者1个缩进符表示。

#### 语法：

	Bash中可以使用echo来进行输出。
		$ echo 'Something'
		$ echo -e '\tSomething\n'

#### 效果：

Bash中可以使用echo来进行输出。

	$ echo 'Something'
	$ echo -e '\tSomething\n'

- - - 

## 强调
Markdown使用`\*`或`\_`表示强调。

#### 语法：

	单星号 = *斜体*
	单下划线 = _斜体_
	双星号 = **加粗**
	双下划线 = __加粗__

#### 效果：
单星号 = *斜体*  
单下划线 = _斜体_  
双星号 = **加粗**  
双下划线 = __加粗__  

- - - 

## 链接
Markdown支持两种风格的链接：*Inline*和*Reference*。 
#### 语法：
*Inline*：以中括号标记显示的链接文本，后面紧跟用小括号包围的链接。如果链接有title属性，则在链接中使用**空格**加**"title属性"**。  
*Reference*：一般应用于多个不同位置使用相同链接。通常分为两个部分，调用部分为`[链接文本][ref]`；定义部分可以出现在文本中的其他位置，格式为`[ref]: http://some/link/address (可选的标题)`。   
*注：ref中不区分大小写。*   

	这是一个Inline[示例](http://equation85.github.com "可选的title")。
	这是一个Reference[示例][ref]。
	[ref]: http://equation85.github.com

#### 效果：

这是一个*Inline*[示例](http://equation85.github.com "可选的title")。   
这是一个*Reference*[示例][ref]。
[ref]: http://equation85.github.com

- - - 

## 图片
图片的使用方法基本上和链接类似，只是在中括号前加**叹号**。   
*注：Markdown不能设置图片大小，如果必须设置则应使用HTML标记&lt;img&gt;。*   
#### 语法：

	Inline示例：![替代文本](/assets/images/jian.jpg "可选的title")
	Reference示例：![替代文本][pic]
	[pic]: /assets/images/ship.jpg "可选的title"
	HTML示例：<img src="/assets/images/jian.jpg" alt="替代文本" title="标题文本" width="200" />

#### 效果：

<img src="/assets/images/jian.jpg" alt="替代文本" title="标题文本" width="200" />

- - - 

## 其他
#### 自动链接
使用**尖括号**，可以为输入的URL或者邮箱自动创建链接。如<test@domain.com>。

#### 分隔线
在一行中使用三个或三个以上的`*`、`-`或`_`可以添加分隔线，其中可以有空白，但是不能有其他字符。

#### 转义字符
Markdown中的转义字符为`\`，可以转义的有：

<ul>
<li>\\ 反斜杠</li>
<li>\` 反引号</li>
<li>\* 星号</li>
<li>\_ 下划线</li>
<li>\{\} 大括号</li>
<li>\[\] 中括号</li>
<li>\(\) 小括号</li>
<li>\# 井号</li>
<li>\+ 加号</li>
<li>\- 减号</li>
<li>\. 英文句号</li>
<li>\! 感叹号</li>
</ul>

- - - 

## 结语
Markdown语法很大程度上减少了编辑的成本，但是在写作这篇文章的时候也发现某些标记对中文的支持似乎并不完美，虽然这些缺陷可以通过直接插入HTML代码解决（但这么做一点都不漂亮）。总的来说，能够在离线状态下使用命令行模式进行写作还是很爽的，相比在线写作模式精力可以更专注。
