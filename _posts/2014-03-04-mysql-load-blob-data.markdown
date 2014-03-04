---
layout: post
title: "MySQL批量导入blob数据"
date: 2014-03-04 20:58
category: Tips
tags: [MySQL]
---
{% include JB/setup %}

在MySQL中可以通过以下的方法导入`blob`类型的数据。

    load data [local] infile '/tmp/a.txt' into table test fields terminated by '\t' (id, @hex_value) set value=UNHEX(@hex_value);

即，将二进制数据转成十六进制字符串，在导入的时候通过`UNHEX`转回来。

`local`需要在启动MySQL命令行时带有参数`--local-infile`, 可以很方便的解决文件找不到的问题。
