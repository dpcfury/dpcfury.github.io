---
title: 搬瓦工KVM安装FinalSpeed优化测试
date: 2019-01-02 10:08:43
categories: 
- 科学上网
- VPS优化
tags: KVM优化
---

教育网环境下自带bbr加速的ss突然速度奇慢，在电信网下表现也较差（不超过2000kbps），于是准备测试在KVM上安装FinalSpeed双端加速测试。

<!-- more -->

![](bbr教育网速度奇慢.png)

### 关闭bbr服务

这里由于我所拥有的这台VPS之前选在安装的系统内核自带了bbr加速，这里先通过配置文件关闭bbr加速，利用vim编辑/etc/sysctl.conf，删除或注释bbr加速相关的两行配置：

```bash
net.core.default_qdisc = fq
net.ipv4.tcp_congestion_control = bbr
```

![](关闭bbr配置.PNG)

```
然后执行 sysctl -p 随后重启VPS
```

ps: 重启过后，ss的链接速度达到了1444kkbps，但是仍然抵挡不止我装双端加速的决心

当然除了上面的关闭bbr加速的方式，还可以更换VPS内核或者直接在搬瓦工控制台更换不带bbr的系统即可

### finalSpeed安装脚本

这里采用91云的一键安装脚本，键入下面的命令，等待安装

```
wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/finalspeed/master/install_fs.sh && bash install_fs.sh
```

![](install_fs.PNG)



安装完成之后最好记录一下fs相关配置的位置，方便之后调试和进一步设置

![](fs_setting.PNG)



fs的安装目录默认为/fs ，日志的路径为/fs/server.log，查看日志可以看到fs监听的tcp与udp端口都为150

### finalSpeed卸载

卸载只需要一键安装脚本命令uninstall：(首先进入到install_fs.sh的下载位置)

```
install_fs.sh uninstall
```

### finalSpeed服务使用

这个一键安装包是91yun完全重写了作者原来的安装代码，启动，停止代码。并加入了服务，可以使用 service finalspeed star | stop 来控制，加入了开机启动启动，fs服务端版本为1.2

| 功能     | 命令                          |
| -------- | ----------------------------- |
| 启动     | /etc/init.d/finalspeed start  |
| 停止     | /etc/init.d/finalspeed stop   |
| 查看状态 | /etc/init.d/finalspeed status |



### finalSpeed 客户端配置

由于fs是一个双端加速，必须配合客户端使用，这里采用的是fs1.2的客户端，1.2的客户端如果不稳定也可选择1.0的客户端

#### windows端下载地址：

[finalspeed_install1.0.exe](https://github.com/91yun/finalspeed/raw/master/finalspeed_install1.0.exe) 

[finalspeed_install1.2.exe](https://github.com/91yun/finalspeed/raw/master/finalspeed_install1.12.exe) 

#### Java版本的客户端地址：

[finalspeed_client1.0.zip](https://raw.githubusercontent.com/91yun/finalspeed/master/finalspeed_client.zip) 

系统需安装 java 运行环境 ,Linux 还需安装 **libpcap**.
Ubuntu,Debian 安装 libpcap: 

```
apt-get -y install libpcap-dev
```

Centos 安装 libpcap: 

```
yum -y install libpcap
```



安装 : 下载解压 .
运行 : 打开终端 , 假设 finalspeed_client.jar 所在路径为 /fsclient , 先切换到该路径 cd /fsclient ,
然后执行 sudo java -jar finalspeed_client.jar , 前面加 sudo, 因为必须以 root 权限运行 , 如果没有 root 权限 , 会无法启用 tcp 协议 .

#### 客户端配置

服务器地址为你VPS的IP地址，传输协议选择TCP或UDP。然后到加速列表选择添加，名称随便起一个就行，加速端口即为你要加速的ss服务端口，本地端口选择一个当前系统未被占用的端口且不常用就行。物理带宽设置严格按照实际带宽设置，否则会导致加速失败：

![](fs_client.PNG)



#### ss客户端设置

![](ss_client.PNG)



以上全部完成之后，进行结果测试，在UDP加速下结果为(教育网)，4k 60帧 最高在23000kbps，加速效果不如openVZ在finalSpeed下的加速：

![](fs_test_udp.PNG)



在TCP协议下（教育网），4k 60 帧最高在36000kbps，但是这个速度极不稳定，一般维持在15000kbps甚至不到1000kbps：

![](fs_tcp.PNG)



（考虑今天的测试可能受学校网络环境影响，2019/01/02，ipV6翻墙又可以用了），后续会在宿舍电信网环境下继续测试。



### bbr与finalSpeed共存

之后，重新启动了bbr加速服务之后，fs在客户端使用UDP协议，测试结果较好，4k环境下能流程运行（秒开）：

![](bbr_fs.PNG)



### 后续

考虑到网络运营商和时段的影响，今天的测试结果不一定能代表大部分情况，后续还会测试KCPtun和KCPServer的加速效果。