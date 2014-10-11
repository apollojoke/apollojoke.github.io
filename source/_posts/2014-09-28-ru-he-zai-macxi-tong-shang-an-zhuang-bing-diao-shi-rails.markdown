---
layout: post
title: "如何在MAC系统上安装并调试Rails"
date: 2014-09-28 20:23:29 +0800
comments: true
categories: ruby rails
---

------------------
###安装ruby和rails

#####安装RVM管理ruby版本  参考 [rvm.io](http://rvm.io)

\curl -sSL https://get.rvm.io | bash -s stable

'rvm get head'    更新RVM

#####安装ruby(2.1.2)

'rvm list known'    可查看所有ruby版本，选择稳定版本安装。

'rvm install 2.1.2'    以2.1.2版本为例，安装ruby。

'rvm --default use 2.1.2'    将此版本的ruby设置为默认版本，避免每次启动shell都要选择所使用的ruby版本。

'rvm list'    可以查看所有安装的ruby版本。

#####安装rails
'gem install rails'    直接安装rails, gem被包含在ruby中，不用单独安装。

#####新建rails工程，并bundle install
'rails new test'    新建名为test的工程。

这个命令执行到最后会执行bundle install。如果长时间卡住，可以 Ctrl + C 结束安装，修改gem源后重新执行。

在工程根目录下Gemfile中，使用淘宝源 'http://ruby.taobao.org' 替换原有gem源。

---------------------------
###美化debug

在Gemfile中添加如下代码：

	group :development do
    	gem 'better_errors'
		gem 'binding_of_caller'
    	gem 'meta_request'
    	gem 'pry'
    	gem 'pry-rails'
	end

另外添加chrome插件 'rails panel',方便调试。