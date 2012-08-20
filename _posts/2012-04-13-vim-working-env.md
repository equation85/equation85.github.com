---
layout: post
title: "Vim IDE for R"
date: 2012-04-13 21:18
category: "技术"
tags: [Linux, Config]
---
{% include JB/setup %}

### 引言
对于需要敲代码的童鞋来说，一个好的IDE往往能够带来事半功倍的效果。之前我所有的算法都是在vim下一行行的敲出来的，好在以往的算法实现起来都比较简单代码量不大，因此一直这么用着也没出什么问题。但是近来发现当一些Map/Reduce算法逻辑比较复杂的时候不太好调试，于是还是希望找一款比较顺手的IDE。

老大推荐了[Eclipse](http://www.eclipse.org "Eclipse")+[StatET](http://www.walware.de/goto/statet "StatET")的组合，StatET将R的运行环境整合到了Eclipse中，同时还能提供自动补全和查看当前环境变量的功能。通过安装SVN插件可以很方便的实现版本控制。对于习惯vim的我来说，如果能够沿用vim的光标控制方式就更好了。显然，和我有着同样想法的人不在少数，很方便的就找到了vim插件。挺爽的，一切操作起来似乎完美无瑕！（除了有点小卡）

一个小时后...

咦？不对！怎么进行文本替换？vim插件貌似只提供了查找的功能，却不能替换。好吧，退而求其次，用它自带的替换功能。Ctrl-F，No！居然弹出了个对话框，也就是说需要经常把手从键盘移到鼠标，这怎么能行！而且更重要的是不支持只对某些行进行替换，无奈还是寻求vim下的解决方案吧。

vim果然不愧为最好用的文本编辑器，通过安装几个插件很容易就能满足上面的需求。可以通过NERDTree来管理工程的代码，通过vim-r-plugin来实现R代码的高亮、自动补全、帮助和运行，通过vcscommand来进行版本控制（支持SVN和GIT），通过FuzzyFinder来实现文件的模糊查找。

罗嗦了这么多，赶紧转入正题吧～

### [NERDTree](http://www.vim.org/scripts/script.php?script_id=1658 "NERDTree") —— 目录管理
#### 常用命令
\:NERDTree打开目录浏览器
- C    切换到光标所在目录
- r    刷新光标所在目录
- u    返回上层目录

### [vim-r-plugin](http://www.vim.org/scripts/script.php?script_id=2628 "vim-r-plugin") —— R插件
依赖[screen](http://www.vim.org/scripts/script.php?script_id=2711 "screen")   
在~/.vimrc中添加   
<pre><code>let g:vimrplugin_screenplugin = 1   "" 是否启用screen插件
let g:vimrplugin_noscreenrc = 1   "" 禁用screenrc
let vimrplugin_buildwait = 240   "" RUpdateObjList的最大等待时间（秒）
let vimrplugin_routmorecolors = 1   "" R输出文件的颜色
let g:vimrplugin_underscore = 0   "" 键入下划线"_"时是否自动转化为赋值号"&lt;-"</code></pre>

#### 常用命令
- \\rf    启动R
- \\rq    退出R
- \\aa    运行整个文件
- \\ff    运行当前函数
- \\d     运行当前行
- \\ss    运行选中文本
- \\rl    显示R控制台中的变量
- \\rp    Print当前对象
- \\rn    Names当前对象
- \\rt    Structure当前对象
- \\xx    注释
- \\;     添加/对齐右端注释
- \\ro    打开/更新对象浏览器
- \:RUpdateObjList    创建当前载入包中函数名的高亮显示
- \:RUpdateObjListAll    创建所有已安装的R包中函数名的高亮显示
- Ctrl-x Ctrl-o    函数名补全（注：可以补全的函数必须在Omni list或者当前.GlobalEnv中）
- Ctrl-x Ctrl-a    显示当前函数的参数列表（注：同上）
- Ctrl-n or Ctrl-p    在自动补全模式的列表中上下移动

### [vcscommand](http://www.vim.org/scripts/script.php?script_id=90 "vcscommand") —— SVN Integration
#### 常用命令
- \:VCSAdd   向源添加文件
- \:VCSAnnotate   在每行代码前显示提交者
- \:VCSCommit   提交对当前文件的更改
- \:VCSDelete   在源中删除当前文件
- \:VCSLog   显示文件的历史更改情况
- \:VCSRevert   回滚当前文件到源中历史最新版本
- \:VCSReview   查看当前文件的某个特定版本
- \:VCSStatus   查看当前文件的版本信息
- \:VCSUnlock   撤销对当前文件的锁定
- \:VCSUpdate   更新当前文件到源中版本
- \:VCSVimDiff   查看当前文件与历史版本的差别

### [FuzzyFinder](http://www.vim.org/scripts/script.php?script_id=1984 "FuzzyFinder") —— 文档查找
依赖[L9 library](http://www.vim.org/scripts/script.php?script_id=3252 "L9 library")   
在~/.vimrc中添加

	nmap ,f :FufFileWithCurrentBufferDir<CR>
	nmap ,b :FufBuffer<CR>
	nmap ,t :FufTaggedFile<CR>

### 效果
<img src="/assets/images/vim-r-sample.png" alt="效果图" title="效果图" width="100%" />

### 结语
每一个用Linux的童鞋都有一颗Geek的心。vim就是这么一款非常小巧的编辑器，而且只要有命令行的地方（自动忽略Windows）就能使用，因此即使在家中通过ssh连接也能够毫无差别的进行使用。和Tmux搭配，不但能够用最少的手指移动来管理多个窗口，而且丝毫不用担心网络连接断开。绝对是居家旅行必备之上品！
