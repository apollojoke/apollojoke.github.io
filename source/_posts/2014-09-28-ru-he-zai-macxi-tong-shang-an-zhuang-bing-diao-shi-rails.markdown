---
layout: post
title: "如何在MAC系统上安装并调试Rails"
date: 2014-09-28 20:23:29 +0800
comments: true
categories: ruby rails
---

------------------

我这辈子第一个程序是VB写的，我第一门真正学习的程序语言是C，但我对我来说，我从Java这个门走进了web开发这个世界。可以说我是一个Java程序员。公司里有个大牛说过（我忘了谁了）：“看一个人适不适合当程序员，就看他能不能接受两种以上不同的程序设计语言。” 所以，我总算开始学Ruby on Rails了。

无论如何，当你已经选择开始做RoR了，就做一个逼格高的RoR开发。如果你买得起HHKB，不用看这边博客，你已经是一个逼格很高的RoR开发了，否则让我这篇博客帮助你。

-------------------

###让VIM成为你的IDE

一个程序员装不装逼，先看他的IDE。

mac上无疑是macvim,然后推荐一个让你macvim变牛逼的工具---[janus]()

janus自带了很多好用的插件例如NERDTree，对于支持Rail的自动补全等，总之很好用。
README中已经写的非常详细，只记录一些最常用的，容易忘的，以便查阅。

1. 在`~/.vim`中执行`rake`就会更新janus；
2. `~/.gvimrc.before` 和 `~/.vimrc.before`两个文件，顾名思义会在`~/.gvimrc`生效前或后生效，可以在这里配置vim；
3. 可以手动创建一个`~/.janus`目录来安装第三方插件；

-------------------

###安装ruby和rails

#####安装RVM管理ruby版本  参考 [rvm.io](http://rvm.io)

\curl -sSL https://get.rvm.io | bash -s stable

'rvm get head'    更新RVM

#####安装ruby(2.1.2)

`rvm list known`    可查看所有ruby版本，选择稳定版本安装。

`rvm install 2.1.2`    以2.1.2版本为例，安装ruby。

`rvm --default use 2.1.2`    将此版本的ruby设置为默认版本，避免每次启动shell都要选择所使用的ruby版本。

`rvm list`    可以查看所有安装的ruby版本。

#####安装rails
`gem install rails`    直接安装rails, gem被包含在ruby中，不用单独安装。

`******`   更新rails

#####新建rails工程，并bundle install
`rails new test`    新建名为test的工程。

这个命令执行到最后会执行bundle install。如果长时间卡住，可以 Ctrl + C 结束安装，修改gem源后重新执行。

在工程根目录下Gemfile中，使用淘宝源 `http://ruby.taobao.org` 替换原有gem源。

---------------------------
###debug的时候都比你同事装逼一点点

在Gemfile中添加如下代码：

	group :development do
    	gem 'better_errors'
		gem 'binding_of_caller'
    	gem 'meta_request'
    	gem 'pry'
    	gem 'pry-rails'
	end

另外添加chrome插件`rails panel`,方便调试。