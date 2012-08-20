---
layout: post
title: "批量文件编码转换工具"
date: 2012-08-20 21:02
category: "工具"
tags: [Linux]
---
{% include JB/setup %}

对于经常要在Linux和Windows中来回切换的人来说，文件编码是件很烦人的事情，于是写了一段小脚本用于批量转换文件编码。

该脚本可以接受通过管道获取的文件名，并逐个进行编码转换，处理过的文件名以其编码名结尾。

For Example:

	ls | fileEncoding -f utf8 -t gbk

该命令的效果是将当前文件夹下的所有文件的编码从utf8转换为gbk。

想试一试吗？[点此](https://github.com/equation85/codebase/blob/master/shell/fileEncoding.sh)获得脚本。:)
