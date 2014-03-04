---
layout: post
title: "Solr中通过DIH从MySQL创建索引"
date: 2012-08-21 20:19
category: Tips
tags: [Solr, "Search Engine"]
---
{% include JB/setup %}

### 准备工作
在利用Solr的DataImportHandler来导入MySQL的数据前，需要MySQL满足一些条件。

1. 运行用户从远程登录，当然如果从本地MySQL数据库创建索引的话可以无视。

修改/etc/mysql/my.cnf：

	bind-address = 0.0.0.0

2. 用户权限，确保用户在username@'%'具有select权限。

以管理员账号登录MySQL，在命令行中运行：

	grant select on database.* to username@'%'; 
	flush privileges;
	select * from mysql.user where user='username';

### 配置Solr
Solr的配置见[官方文档](http://wiki.apache.org/solr/DataImportHandler)即可。

