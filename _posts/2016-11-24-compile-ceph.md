---
layout: post
title: 源码编译Ceph
date: 2016-11-24
category: "Ceph"
---


Ceph从搭建开始接触

## 环境准备

通道机外网ip 121.201.53.17 内网ip 192.168.0.11

| hostname      | OS         | ip           | CPU 线程核                          | Memory | role      |
| ------------- | ---------- | ------------ | ---------------------------------- | ------ | --------- |
| cephfs-client | CentOS 7.2 | 192.168.0.15 | 2*Intel Xeon E312xx (Sandy Bridge) | 4G     | cli       |
| ceph-1        | CentOS 7.2 | 192.168.0.12 | 2*Intel Xeon E312xx (Sandy Bridge) | 4G     | mon / osd |
| ceph-2        | CentOS 7.2 | 192.168.0.14 | 2*Intel Xeon E312xx (Sandy Bridge) | 4G     | mon / osd |
| ceph-3        | CentOS 7.2 | 192.168.0.13 | 2*Intel Xeon E312xx (Sandy Bridge) | 4G     | mon / osd |


将github上的代码clone到本地，我选取的是较稳定的10.2.3版本。

```bash
git clone -b v10.2.3 https://github.com/ceph/ceph.git
```
然后准备安装依赖
> The list of Debian or RPM packages dependencies can be installed with:

```bash
cd ceph
./install-deps.sh
```

尝试使用cmake编译失败
cmake命令执行时或是 ./run-cmake-check.sh 时报错
install TARGETS given no RUNTIME DESTINATION for executable target "mount.ceph".
install PROGRAMS given no DESTINATION!
猜测是该版本的源码对cmake的支持不好，需要有手动配置的地方，后面有时间再看看。

采用make编译源码
```bash
./autogen.sh
./configure --prefix=/usr/bin --libdir=/usr/lib64 --without-radosgw --without-spdk --without-kinetic --without-libzfs
make j2
```