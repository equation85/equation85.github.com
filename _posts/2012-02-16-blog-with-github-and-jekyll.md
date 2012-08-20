---
layout: post
title: "Github+Jekyll构建个人博客"
comments: true
date: 2012-02-16 19:57
categories: "技术"
tags: Other
---

毕业有一年了，很久之前就萌生了写博客的念头，希望将学习和工作中的点滴记录成文字，但由于比较懒又不喜欢博客类网站提供的写作环境，故而一直未能真正动手进行。最近恰逢工作变动，于是对各种博客工具进行了大致的比较，最终决定采用[Github](http://github.com)+[Jekyll](http://jekyllbootstrap.com/lessons/jekyll-introduction.html)的方式来构建个人博客。   
使用Jekyll的好处：

<ul>
<li>可以在各种文字编辑器中通过Markdown或者Textile来写博文。</li>
<li>可以在本地预览博客。</li>
<li>写作无需Internet连接。</li>
<li>可以通过Git进行发布。</li>
<li>可以在静态Web服务器上构建博客站点。</li>
<li>可以通过Github Pages服务免费构建博客。</li>
<li>无需数据库。</li>
</ul>

本文将简要介绍如何在Github上建立个人博客。

## Github
1\. 首先，需要的是Github帐号，如果没有的话[在这里注册](https://github.com/plans)。  
2\. 在自己的机器上配置Git，教程见[这里](http://help.github.com/linux-set-up-git/)。  
完成以上两步后，就可以用Jekyll来构建写作环境了。

## Jekyll
Jekyll是一个静态网站生成工具。它允许用户使用HTML、Markdown或Textile来建立静态页面，然后通过模板引擎Liquid（[Liquid Templating Engine](http://liquidmarkup.org)）来运行。  
### 1. 安装Jekyll
通过RubyGems安装

	$ sudo gem install jekyll

编译插件需要ruby1.9.1-dev

	$ sudo apt-get install ruby1.9.1-dev

### 2. 创建Repo
在Github Dashboard中创建一个新的名为USERNAME.github.com的Repo。

### 3. 安装Jekyll-Bootstrap
	$ git clone https://github.com/plusjade/jekyll-bootstrap.git USERNAME.github.com
	$ cd USERNAME.github.com
	$ git remote add origin git@github.com:USERNAME/USERNAME.github.com.git
	$ git push origin master

### 4. 安装rake
	$ sudo gem install rake

### 5. 配置\_config.yml
在_config.yml中除了可以设置博客的常规信息之外，还默认自带了一些常用功能，如评论（disqus）、Analytics跟踪（Google Analytics）等等。详细教程见[这里](http://jekyllbootstrap.com/usage/blog-configuration.html)。

以上配置完成之后可以很容易的通过  

	$ rake post title="Hello World"
	$ rake page name="about.markdown"

分别来创建博文和页面。

