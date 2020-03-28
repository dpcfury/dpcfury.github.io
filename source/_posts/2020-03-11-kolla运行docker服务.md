---
title: kolla运行docker服务
date: 2020-03-11 20:40:18
urlname: kolla-start-docker
categories:
- docekr
tags:
- docker
- kolla
---
> kolla制作的docker镜像，通过juju部署服务，发现容器在juju中的状态一直是重启状态，检查启动日志，成功运行了进程脚本，于是从镜像入手进行分析。
<!--more-->

### kolla运行docker
 通过kolla启动docekr，运行的是一个启动命令脚本，作为docker的1号进程，而docker判断容器是否运行的依据就是这个启动进程是否存在，即docker中存在的前台和后台概念。
 按照这种道理，需要在启动命令中直接进行容器中服务的启动，但是初次之外，还有个trick操作。


### 挂起pid=1的进程

 即在启动命令脚本的最后，加入一行：
 ```bash
 tail -f >/dev/null 2>&1
 ```
 其原理便是，通过tail和top这种挂起的服务，保持pid=1的进程不会结束，从而实现docker容器的后台运行。

