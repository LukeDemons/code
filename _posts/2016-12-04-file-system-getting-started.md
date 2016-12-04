---
layout: post
title: 文件系统学习
date: 2016-12-04
category: "File System"
---


了解基本文件系统知识

## 文件系统学习

### 概要

命名文件及放置文件的逻辑存储和恢复的系统。

### 磁盘知识 

hdd结构：
物理上，一块磁盘，有多个盘片。每个盘片上根据半径不同，有很多磁道（track）。不同盘片上半径相同的磁道，在逻辑上称为磁柱（cylinder）。一个磁道上的某段弧内的区域，被称为扇区（sector）。每个扇区的大小是512字节。

容量公式： 
磁盘容量 = 512字节 x每个磁道的扇区数 x 磁头数 x柱面数
磁盘容量 = 512字节 x每个磁道的扇区数 x 盘片数 x每个盘片的磁道数
注意：磁头数 = 盘片数，柱面数 =每个盘片的磁道数。

### 分区及逻辑结构：
MBR：Master Boot Recorder，是整个磁盘的第0个柱面，记录有整个磁盘的分区信息。开机后对磁盘的读取首先从MBR开始。如果一个磁盘的MBR坏了，那么整个磁盘也就废了。

Superblock：每个分区（partition）起始位置的那个block，存储着这个分区的信息，包括分区的块总数、空闲块总数、非空闲块总数、所有块的状态信息映射表等。对某个分区中的数据进行读写时，首先读取的是该分区的superblock。如果一个分区的superblock坏了，那么整个分区也就废了（但不影响其他分区）。

### 读写过程：
先读整个磁盘的MBR，获取所有分区信息，并确定要载入的操作系统文件所在的分区。然后，读取某个分区时，先读取该分区的superblock，然后找到相应的块（可能是多个）进行读取。

## 文件系统

![filesystem-architecture](images/2016/filesystem-architecture.png "filesystem-architecture")<br>

1.	用户层：最上面用户层就是我们日常使用的各种程序，需要的接口主要是文件的创建、删除、打开、关闭、写、读等。
2.  VFS层：我们知道Linux分为用户态和内核态，用户态请求硬件资源需要调用System Call通过内核态去实现。用户的这些文件相关操作都有对应的System Call函数接口，接口调用 VFS对应的函数。
3.  文件系统层：不同的文件系统实现了VFS的这些函数，通过指针注册到VFS里面。所以，用户的操作通过VFS转到各种文件系统。文件系统把文件读写命令转化为对磁盘LBA的操作，起了一个翻译和磁盘管理的作用。
4.  缓存层：文件系统底下有缓存，Page Cache，加速性能。对磁盘LBA的读写数据缓存到这里。
5.  块设备层：块设备接口Block Device是用来访问磁盘LBA的层级，读写命令组合之后插入到命令队列，磁盘的驱动从队列读命令执行。Linux设计了电梯算法等对很多LBA的读写进行优化排序，尽量把连续地址放在一起。
6.  磁盘驱动层：磁盘的驱动程序把对LBA的读写命令转化为各自的协议，比如变成ATA命令，SCSI命令，或者是自己硬件可以识别的自定义命令，发送给磁盘控制器。Host Based SSD甚至在块设备层和磁盘驱动层实现了FTL，变成对Flash芯片的操作。
7.  磁盘物理层：读写物理数据到磁盘介质。

### EXT4 文件结构示例：

![filesystem-structure](images/2016/filesystem-structure.png "filesystem-structure")<br>

访问文件的时候，用户发下来文件路径Dentry（/var/test.txt），VFS通过Hash的方法直接通过路径查到最终Dentry，找到inode，来直接查找一个路径，而不是一级级翻下来。

### 其他：
在内存中，每个文件都有一个dentry(目录项)和inode(索引节点)结构，dentry记录文件名、父目录、子目录等信息，形成文件树结构。inode结构则包含文件的宿主、创建时间和在存储介质上的位置和分布等信息。其中每个dentry都有一个唯一的inode，而每个inode则可能有多个dentry，这种情况是由ln硬链接产生的。

硬链接：其实就是同一个文件具有多个别名，具有相同inode，而dentry不同。

软链接：软链接具有自己的inode，即具有自己的文件，只是这个文件中存放的内容是另一个文件的路径名。因此软链接具有自己的inode号以及用户数据块。

系统读取外存中的inode和dentry信息进行一定加工后，生成内存中的inode和dentry。

整理自：[http://mp.weixin.qq.com/s/UP0OAdb_VThWXO9R4lE7mQ](http://mp.weixin.qq.com/s/UP0OAdb_VThWXO9R4lE7mQ "饿想她")