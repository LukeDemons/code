---
layout: post
title: 在Ubuntu14.04上搭建devstack环境
date: 2015-04-02
category: "OpenStack"
---


最简脚本搭建OpenStack环境

OpenStack的官方文档是配置多个节点的，如果按照[openstack-install-guide-apt-juno.pdf](http://docs.openstack.org/juno/install-guide/install/apt/content/)安装会很麻烦，本人在nova部分的安装时nova.conf配置文件格式的出错了，以后再搞，先说devstack。

*注:devstack适用于实验环境而非生产环境*

## 环境

操作系统：Ubuntu14.04

## 安装步骤

##### 0.换镜像源

修改/etc/apt/sources.list 换成国内163的镜像源。

##### 1.系统升级

{% highlight javascript %}
sudo apt-get update;
sudo apt-get dist-upgrade;
sudo apt-get upgrade;
{% endhighlight %}

##### 2.修改pip源

{% highlight javascript %}
    vi ~/.pip/pip.conf        # 安装pip不会覆盖此文件
{% endhighlight %}
###### 键入以下代码

>[global]<br>
>index-url=http://pypi.douban.com/simple

##### 3.安装git *已安装的可忽略此步*

{% highlight javascript %}
sudo apt-get install git
{% endhighlight %}

##### 4.克隆devstack至本机

{% highlight javascript %}
git clone git://github.com/openstack-dev/devstack.git
{% endhighlight %}

##### 5.进入devstack目录

{% highlight javascript %}
cd devstack/
{% endhighlight %}

##### 6.执行.stack.sh脚本下载安装最新版本的OpenStack

{% highlight javascript %}
./stack.sh
{% endhighlight %}
期间需设定5次密码，为简单起见将各个组件的密码设置成一样的就好。