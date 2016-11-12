---
layout: post
title: Ubuntu+Github Pages+Jekyll实验环境搭建
date: 2015-03-05
category: "Linux"
---


Ubuntu搭建本地Jekyll环境


> 根据[TNW的报道](http://thenextweb.com/insider/2013/04/07/github-moves-pages-from-domain-over-security-concerns/)，出于网站安全考虑，软件开发服务 GitHub 将自有服务器上 GitHub Pages 内容转移到一个新的域名，github.io 。

安装ruby环境

    sudo apt-get install ruby ruby-dev

换国内淘宝ruby源

    sudo gem sources –r https://rubygems.org/
    sudo gem sources –r http://rubygems.org/
    sudo gem sources -a http://ruby.taobao.org/

查看源是否替换成功

    sudo gem sources -l

显示

    *** CURRENT SOURCES ***

    http://ruby.taobao.org/

安装Jekyll和Markdown支持

    sudo gem install jekyll
	sudo gem install rdiscount

安装js环境

	sudo apt-get install nodejs

新建git repo或fork一份github中demo
clone至本地 cd blog目录 开启jekyll服务

	jekyll serve

延伸阅读
	[markdown手册](http://wowubuntu.com/markdown/)
	[搭建github免费博客](http://baidut.github.io/howto/2015/04/13/%E6%90%AD%E5%BB%BAgithub%E5%85%8D%E8%B4%B9%E5%8D%9A%E5%AE%A2/)
