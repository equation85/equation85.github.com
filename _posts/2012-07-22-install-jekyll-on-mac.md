---
layout: post
title: "在Mac中部署你的Jekyll"
date: 2012-07-22 07:59
category: "备忘"
tags: [Mac, Config]
---
{% include JB/setup %}

关于Jekyll，可以参考我的[第一篇博文](/blog/blog-with-github-and-jekyll/)。之前在Ubuntu中配置的时候比较幸运，在不甚了了的情况下就正常跑通了。如今切换到Mac下，自然还是希望能知其所以然。

在Mac下部署Jekyll需要以下几个组件：**xcode**, **git**, **rvm**, **ruby**

#### Xcode, Git
见[上一篇博文](/blog/customize-terminal-on-mac/)。

#### RVM
RVM被用来管理系统中不同的Ruby版本。强烈建议在安装前[进行了解](https://rvm.io/rvm/install/)。由于RVM是在bash下运行的，所以如果不是以bash开启终端的首先必须切换过来。

安装稳定版

	curl -L https://get.rvm.io | bash -s stable

注意安装过程中输出的信息，会提示如何启用rvm，如

	source ~/.rvm/scripts/rvm

当然也可以在打开终端时自动加载，在~/.bash_profile最后添加

	[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm"

#### Ruby
<ol>
<li>
在RVM中安装Ruby 1.9.2

<pre>
rvm install 1.9.2
</pre>

在我的配置下报错“The provided compiler '/usr/bin/gcc' is LLVM based, it is not yet fully supported by ruby and gems, please read \`rvm requirements\`.”
是因为需要非LLVM的gcc，而这在xcode 4.3+中被取消了。

对于这个问题可以参考<a href="http://blog.yorkxin.org/2012/03/09/ruby-192-with-xcode-43">资料1</a>，<a href="http://stackoverflow.com/questions/8032824/cant-install-ruby-under-lion-with-rvm-gcc-issues">资料2</a>，<a href="https://github.com/kennethreitz/osx-gcc-installer/downloads">所用gcc下载地址</a>。
</li>

<li>
或者未避免麻烦也可以安装Ruby 1.9.3，至少我没有碰到安装或使用中的问题

<pre>
rvm install 1.9.3
</pre>

启用Ruby 1.9.3

<pre>
rvm use 1.9.3
</pre>
</li>
</ol>

#### Jekyll
通过RubyGems安装

	gem install jekyll
	gem install jekyll-tagging

### 附
参考：http://brandonbohling.com/2011/08/27/Installing-Jekyll-on-Mac/
