---
layout: post
title: "R和Java的整合"
date: 2012-09-09 19:50
category: "技术"
tags: [Java, R]
---
{% include JB/setup %}

作为统计工具R有着无可比拟的灵活性和社区支持，而在构建系统方面Java的优势也毋庸置疑的，那怎样将二者结合起来取长补短以获得更大的灵活性呢？本文简单介绍通过rJava包来实现R和Java之间的互通。

### Calling Java code from R
在R中调用Java基本没有什么大问题，只要安装好rJava包之后，就可以分别通过`.jnew`和`.jcall`来创建Java类以及调用类方法了。详细可参考文档[rJava](http://www.rforge.net/rJava/)。

值得记住的几个地方是JNI类型的表示：

- I = integer
- D = double (numeric)
- J = long
- F = float
- V = void
- Z = boolean
- C = char (integer)
- B = byte (raw)
- L&lt;class&gt;; 表示Java中的类&lt;class&gt;，譬如“Ljava/lang/Object;”
- [&lt;type&gt; 表示&lt;type&gt;类型的数组对象，譬如“[D”表示double型的数组

**注：由于在R中不存在long或float相应的类型，因此这两种类型都会被转换成numeric类型。如果要向Java中传递这两种类型，则可以使用函数.jlong和.jfloat。**

### Calling R code from Java
通过[JRI](http://www.rforge.net/JRI/)接口可以实现在Java中调用R代码（目前该接口是rJava包的一部分）。

1. 将JRI.jar、JRIEngine.jar、REngine.jar添加到Java的CLASSPATH中去。

2. 在`java.library.path`路径下包含libjri.jnilib，否则会报错`Cannot find JRI native library!`。

3. 另外还需设置环境变量`R_HOME`：如果是在Eclipse下使用，则可以在“Run configure”的Environment标签下添加环境变量“R\_HOME”；如果是在脚本中运行，则只需提前`export R_HOME`即可。到这里自带的两个示例程序实际上就可以运行了。

4. 另外，根据程序的需求可能还需添加其他环境变量如：LD\_LIBRARY\_PATH、R\_SHARE\_DIR、R\_INCLUDE\_DIR、R\_DOC\_DIR，具体可视情况参照自带的run脚本添加即可。

如何使用可参见自带的rtest.java和rtest2.java。

**资料：**[Javadoc文档](http://www.rforge.net/org/docs/)

