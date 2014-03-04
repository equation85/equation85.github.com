---
layout: post
title: "[转] Ubuntu下用GAE做goAgent代理"
date: 2012-04-15 16:02
category: "备忘"
tags: [Linux, goAgent]
---
{% include JB/setup %}

**本文仅为存档目的，如有问题请参考[原文](http://www.kafaafa.info/archives/146226 "原文")。**

早就听说Google+的同学们说goAgent了，正好我前些日子使用Ubuntu作为自己日常使用的操作系统。于是就整理一下，做一个教程，给自己存档也方便给其他同学看看。 关于如何申请GAE我就不另赘言了。

### 先决条件：
1. 获得ubuntu的python环境，打开终端输入，`sudo apt-get install python`

2. 下载[goAgent](http://code.google.com/p/goagent/ "goAgent")，可以顺便把chrome的插件SwitchySharp一道装上，顺带把https://raw.github.com/phus/phus-config/master/SwitchyOptions.bak上传到switchysharp。

3. 下载[GAE for linux](http://code.google.com/appengine/downloads.html "GAE for linux")，选择linux版本即可。

### 一.服务端的上传
1.在ubuntu下使用GAE上传goAgent，将goAgent放到google_appengine目录下，并在终端输入`cd /home/yourusername/google_appengine`（你google_appengine的绝对路径）。

2.在你的goAgent的server目录里，有一个文件app.yaml，用文本编辑器打开，填入你在GAE的ID，和version(默认是1)，保存。 3.在终端，`cd /home/yourusername/google_appengine`（你google_appengine的绝对路径）后，输入`sudo python appcfg.py update goAgent/server/python`，填入你的email和密码就能上传了。

### 二.客户端的使用
1.在goAgent的local目录中，有一个proxy.ini文件，将你的appid填入。 2.终端进入到你的local目录，例如`cd /home/yourusername/google_appengine/goAgent/local`，运行`sudo python proxy.py`。好了，现在你可以运用以上的方式通过Ubuntu在chromium浏览器使用goAgent进行翻墙了。

### 三.关于快捷方式的使用
很多应该和我一样，在linux下中一些常用的软件常常需要在终端输入命令觉得很繁琐，想通过快捷方式直接点击运行。

详细方法如下：

1. 创建快捷方式：`sudo gedit /usr/share/applications/goAgent.desktop`   
在文本编辑器里输入

<pre><code>[Desktop Entry] 
Name = goAgent 
Comment = a proxy tool 
Exec = /home/YourUserName/google_appengine/goAgent/local/proxy.py #proxy.py的绝对路径# 
Icon = /home/kafaafa/google_appengine/goAgent/local/goagent.png #图标的绝对路径# 
Terminal = true #使用终端运行# 
Type = Application 
Categories = Application;Development; #放在软件–开发者目录中#</code></pre>

2. 现在需要给快捷方式以sudo超级用户方式运行，否则点击会一闪而过。当然你也可以不让他在终端显示，只需要将Terminal=true改成Terminal=false 

<pre><code>sudo chmod 777 /home/kafaafa/App/google_appengine/goAgent/local/proxy.py</code></pre>

