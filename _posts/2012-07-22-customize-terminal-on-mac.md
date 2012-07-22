---
layout: post
title: "定制你的Mac终端"
date: 2012-07-22 07:56
category: "备忘"
tags: [Mac, Config]
---
{% include JB/setup %}

虽然苹果素来以精美的界面和优质的体验闻名，但是却不得不说这OS X下的终端也太简陋了。不过幸好系统是基于盛产Geek的UNIX而来的，所以总是有办法解决的（这里鄙视下“某逗死”系统）。

好吧，言归正传，就简单记录一下我的配置以备不时之需。

### Terminal
作为一名半Geek，首先要搞定的当然是命令行了。

#### 用bash作为默认的shell
额，这个就跳过吧，对命令行有要求的人不可能不知道的。

#### 自定义提示符
默认的提示符又长又别扭，还是喜欢改成Ubuntu的那种风格。在~/.bash_profile（不是.bashrc了）中添加：

	export PS1="\u@\h:\w $"

需要另外设置颜色的话可以参考[这个](http://blog.superuser.com/2011/09/21/customizing-your-bash-command-prompt/)和[那个](http://www.mactricksandtips.com/2008/10/customizing-the-mac-terminal-bash-prompt.html)。

#### ls和grep的高亮
向~/.bash_profile中加入

	export CLICOLOR=1
	export GREP_OPTIONS="--color=auto"

#### 设置终端的主题
到这里就已经有了一个彩色的终端了，然而光有颜色还是不够的。只有精美的字体和搭配良好的颜色才能使命令行显得不是那么的枯燥乏味，因此还需要一个好的主题。这里推荐使用[Solarized](http://ethanschoonover.com/solarized)。

对于Lion的用户，只需下载[两个terminal](https://github.com/altercation/solarized/tree/master/osx-terminal.app-colors-solarized)文件，双击安装就可以了。然后只要在终端的Preferences中选择相应的主题为默认。


### Vim
作为终端用户，Vim肯定是另一件不可缺少的神器。我们同样可以使用Solarized主题让它变得更华丽些。

<ol>
<li>
下载安装<a href="https://github.com/tpope/vim-pathogen">Pathogen</a>。

<pre>
$ mkdir -p ~/.vim/autoload ~/.vim/bundle; \
>    curl -so ~/.vim/autoload/pathogen.vim \
>    http://raw.github.com/tpope/vim-pathogen/master/autoload/pathogen.vim
</pre>

并向~/.vimrc中添加：

<pre>
call pathogen#infect()
</pre>
</li>

<li>
下载安装主题

<pre>
$ cd ~/.vim/bundle
$ git clone git://github.com/altercation/vim-colors-solarized.git
</pre>
</li>

<li>
修改~/.vimrc

<pre>
syntax enable
set background=dark
colorscheme solarized
</pre>
</li>

</ol>

更多关于终端的设置，见[这里](http://ethanschoonover.com/solarized/vim-colors-solarized)。

#### Tmux
在命令行下tmux的好处就不多说了，谁用谁知道。

	sudo port install tmux

### Git
Mac OS X中默认是没有Git的，所以需要自行安装。至少有以下三种方式可供选择：

1. 选择GUI的[git.app](https://central.github.com/mac/latest)。有图形化界面，安装和操作非常简单。
2. 使用MacPort下载安装。[MacPort](http://www.macports.org/)是一个开源的在Mac OS中用于编译、安装、升级开源软件的包管理工具，类似apt和yum。

<pre>
sudo port selfupdate
port search git
sudo port install git-core
</pre>

3. 从源码安装，详细见[这里](http://blog.maxiang.net/install-git-on-mac/63/)。

#### MacPort
MacPort只需从其主页上[下载相应的pkg](http://www.macports.org/install.php)进行安装即可。

#### Xcode
利用MacPort编译git-core需要xcode。

1. 将Apple ID注册为开发者。
2. 下载安装Xcode 4.3.3 for Lion。
3. 下载安装Command Line Tools for Xcode。
4. 切换当前的xcode

<pre>
sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer/
</pre>

