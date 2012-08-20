---
layout: post
title: "Mac上Tomcat6的安装"
date: 2012-08-20 21:59
category: "备忘"
tags: [Mac, Config, Tomcat]
---
{% include JB/setup %}

### 步骤
1. 从[官方](http://tomcat.apache.org/download-60.cgi)下载Tomcat6的二进制包。

2. 在/Library中创建相应的目录，并解压至该目录下。

<pre>
	cd /Library
	mkdir Tomcat
	chown username Tomcat
	chgrp admin Tomcat
	cd Tomcat
	unzip ~/Download/apache-tomcat-6.0.35.zip
	ln -sfvh apache-tomcat-6.0.35 Home
</pre>

3. 编辑配置文件

Home/conf/server.xml中配置端口：

	<Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443"/>

Home/conf/tomcat-user.xml中配置Tomcat管理员账户：

	<role rolename="manager"/>
	<user username="admin" password="password" roles="standard,manager,admin"/>
	
4. 手动启动

	cd Home/bin
	rm *.bat
	chmod 750 *.sh
	./startup.sh   # 启动
	#./shutdown.sh   # 停止

可访问http://localhost:8080看是否成功启动。

### 将Tomcat作为服务启动
由于我目前不需要在自己电脑上作为服务开启，故这部分正确性未知。

1. 创建脚本$TOMCAT_HOME/bin/tomcat-launchd.sh

<pre>
	#!/bin/sh
	#
	# Wrapper for running Tomcat under launchd
	# Required because launchd needs a non-daemonizing process

	function shutdown() {
		$CATALINA_HOME/bin/shutdown.sh
		/bin/rm $CATALINA_PID
	}

	function wait_for_death() {
	    while /bin/kill -0 $1 2> /dev/null ; 
		do
			sleep 2 
		done
	}

	export CATALINA_PID=$CATALINA_HOME/logs/tomcat.pid
	$CATALINA_HOME/bin/startup.sh
	trap shutdown QUIT ABRT KILL ALRM TERM TSTP
	sleep 2
	wait_for_death `cat $CATALINA_PID`
</pre>

2. 创建文件/Library/LaunchDaemon/org.apache.tomcat.plist

<pre>
	&lt;?xml version="1.0" encoding="UTF-8"?&gt;
	&lt;!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" 
	      "http://www.apple.com/DTDs/PropertyList-1.0.dtd"&gt;
	&lt;plist version="1.0"&gt;
	&lt;dict&gt;
		&lt;key&gt;Label&lt;/key&gt;
		&lt;string&gt;org.apache.tomcat&lt;/string&gt;
		&lt;false/&gt;
		&lt;key&gt;OnDemand&lt;/key&gt;
		&lt;false/&gt;
		&lt;key&gt;RunAtLoad&lt;/key&gt;
		&lt;true/&gt;
		&lt;key&gt;ProgramArguments&lt;/key&gt;
		&lt;array&gt;
			&lt;string&gt;/Library/Tomcat/Home/bin/tomcat-launchd.sh&lt;/string&gt;
		&lt;/array&gt;
		&lt;key&gt;EnvironmentVariables&lt;/key&gt;
		&lt;dict&gt;
			&lt;key&gt;CATALINA_HOME&lt;/key&gt;
			&lt;string&gt;/Library/Tomcat/Home&lt;/string&gt;
			&lt;key&gt;JAVA_OPTS&lt;/key&gt;
			&lt;string&gt;-Djava.awt.headless=true&lt;/string&gt;
		&lt;/dict&gt;
		&lt;key&gt;StandardErrorPath&lt;/key&gt;
		&lt;string&gt;/Library/Tomcat/Home/logs/tomcat-launchd.stderr&lt;/string&gt;
		&lt;key&gt;StandardOutPath&lt;/key&gt;
		&lt;string&gt;/Library/Tomcat/Home/logs/tomcat-launchd.stdout&lt;/string&gt;
		&lt;key&gt;UserName&lt;/key&gt;
		&lt;string&gt;_appserver&lt;/string&gt;
	&lt;/dict&gt;
	&lt;/plist&gt;
</pre>

3. 加载启动进程

	sudo launchctl load /Library/LaunchDaemon/org.apache.tomcat.plist

其中**load**处可以用子命令**unload**，**stop**，**start**代替。由于在作为服务启动时是以**\_appserver**用户运行的，所以要确保文件的访问权限。

### 参考：
1. [http://code.google.com/p/gbif-providertoolkit/wiki/TomcatInstallationMacOSX](http://code.google.com/p/gbif-providertoolkit/wiki/TomcatInstallationMacOSX)
2. [http://code.google.com/p/gbif-providertoolkit/wiki/PermissionSettings](http://code.google.com/p/gbif-providertoolkit/wiki/PermissionSettings)

