---
layout: post
title: "Ubuntu 11.10中安装jdk"
date: 2012-02-20 16:58
category: "技术"
tags: [Linux, Java, Config]
---
{% include JB/setup %}

将Ubuntu升级到11.10后，发现源里面没有sun-java6-jdk，所以无法通过apt-get来进行安装，只能手动进行。

过程如下：

#### 下载
首先去Oracle网站下载jdk，`jdk-6u29-linux-i586.bin`。

#### 创建Java目录
	$ sudo cp jdk-6u29-linux-i586.bin /usr/lib/jvm
	$ cd /usr/lib/jvm
	$ sudo ./jdk-6u29-linux-i586.bin
	$ sudo ln -s jdk1.6.0_29 java-6-sun
	$ sudo rm jdk-6u29-linux-i586.bin

#### 设置环境变量
	$ sudo vim /etc/enviroment
	PATH中添加/usr/lib/jvm/java-6-sun/bin
	添加
	JAVA_HOME="/usr/lib/jvm/java-6-sun"
	JRE_HOME="$JAVA_HOME/jre"
	CLASSPATH=".:$JAVA_HOME/lib:$JRE_HOME/lib"

#### 选择alternative
最后就是告诉系统使用sun的jdk，而不是open jdk：

	$ sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/java-6-sun/bin/java 300
	$ sudo update-alternatives --install /usr/bin/javac javac /usr/lib/java/java-6-sun/bin/javac 300
	$ sudo update-alternatives --config java  #在出现的菜单中选择sun jdk即可

#### 验证当前的java版本
	$ java -version
	java version "1.6.0_29"
	Java(TM) SE Runtime Environment (build 1.6.0_29-b11)
	Java HotSpot(TM) Server VM (build 20.4-b02, mixed mode)

