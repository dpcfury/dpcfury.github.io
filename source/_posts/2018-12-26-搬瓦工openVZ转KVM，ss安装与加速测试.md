---
title: 搬瓦工openVZ转KVM，ss安装与加速测试
date: 2018-12-26 20:04:24
categories: 
- 科学上网
- VPS优化
tags: ss安装
toc: true
---
### VPS购买与系统安装

> 更换新的vps原因是搬瓦工前天发出公告宣布 OpenVZ 系列的所有 VPS 都已经不支持续费了 ,原先有一台LA基于openVZ架构的VPS，安装shadowSocks-libev与finalspeed加速使用起来还不错，缺陷就是不能在内核加速，这次顺势调整到KVM架构的主机。

<!-- more -->

#### 机房选择

目前搬瓦工提供的方案中按速度排行 HK PCCW  > CN2 GIA > CN2 GT >  普通KVM，但是价格而言HK PCCW较为奢侈，CN2 GIA 价格也较贵，目前存货中性价比较高的是 DC8 CN2 机房的普通CN2方案，[搬瓦工库存](https://stock.bwg.net/ " 搬瓦工库存")，目前提供的方案已经少有月付，可以订阅CN2 GIA的\$39.99 补货通知，当前情况还有一个星期就要回收VPS，无奈选择\$ 29.99的 CN2 方案。 

![1545807890085](vps.png)

注意location选择的是DC8 ，目前相传是CN2测试较好的机房，可用支付宝付款，在结账页面可输入优惠码BWH26FXH3HIQ ，能有6.25%的优惠（目前最高），如果优惠码过期可参考[最新优惠码](https://www.bwgyhw.com/bandwagonhost-lastest-promo/ "最新优惠码")，总计花费人民币195左右，购买完成稍等一段初始化时间即可去My Services中查看和进行后续安装操作。

![1545808631772](vps_list.png)



#### 系统安装

目前搬瓦工已经在系统上直接继承了BBR加速，在这里选择自带bbr加速的64位CentOs7，也可选择不带bbr的CentOs7再后续手动安装，选则内置的bbr加速防止内核不兼容问题。

![1545809335977](install_os.png)

xshell远程登录主机查看bbr是否安装

```bash
lsmod | grep bbr
```

![1545813768403](ls_bbr.png)



### Shadowsocks服务端安装



#### 安装脚本（四种版本）

首先要明确一点，不管 Shadowsocks 有几种版本，都分为服务端和客户端，服务端是部署在服务器（VPS）上的，客户端是在你的电脑上使用的。 Shadowsocks 服务端大体上有 4 种版本，按照程序语言划分，分别为 Python ，libev ，Go ， Nodejs ，目前主流使用前 3 种，具体不同版本的feature可参考《[Feature Comparison across Different Versions](https://github.com/shadowsocks/shadowsocks/wiki/Feature-Comparison-across-Different-Versions "不同ss版本比较")》。

这里我选用的是[秋水逸冰](https://teddysun.com/)制作的四合一一键安装脚本，选择其中的shadowsocks-libev（之前使用过感觉较好）。



#### 关于该脚本（感谢作者了哇）

1、一键安装 Shadowsocks-Python， ShadowsocksR， Shadowsocks-Go， Shadowsocks-libev 版（四选一）服务端； 

2、各版本的启动脚本及配置文件名不再重合； 

3、每次运行可安装一种版本，libev版本的多开需要多个实例；

 4、支持以多次运行来安装多个版本，且各个版本可以共存（注意端口号需设成不同）； 5、若已安装多个版本，则卸载时也需多次运行（每次卸载一种）； 



#### 默认配置

| 参数        | 默认值                             |
| ----------- | ---------------------------------- |
| Server IP   | 默认VPS IP即可                     |
| Server Port | 默认9000-19999之间随机             |
| password    | 用户自己设定                       |
| 加密方式    | 用户自己设定，libev默认aes-256-gcm |
| 协议        | 用户自己设定，默认为origin         |



#### 客户端下载（可能有时会需要fan qiang）

常规版 Windows 客户端

 [<https://github.com/shadowsocks/shadowsocks-windows/releases> ](<https://github.com/shadowsocks/shadowsocks-windows/releases> )

ShadowsocksR 版 Windows 客户端

 [<https://github.com/shadowsocksrr/shadowsocksr-csharp/releases> ](<https://github.com/shadowsocksrr/shadowsocksr-csharp/releases> )



#### 脚本安装

root用户登录，下载脚本并赋予执行权限，最后安装脚本

```
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```

![1545819349492](select_version.png)

随后根据脚本设置三个参数：

1、连接密码
2、连接端口号（程序随机分配一个端口号，可自定义）
3、加密方法（建议选择第7个，aes-256-cfb）

![1545819377748](set_pwd.png)

![1545819771508](set_port.png)

![1545819881885](set_method.png)

加密方式建议选择7) aes-256-cfb，然后剩下的就一路enter即可，大概需要几分钟，完成之后提示如下

```
Congratulations, Shadowsocks-libev server install completed!
Your Server IP        :  your-server-ip 
Your Server Port      :  your-server-port
Your Password         :  your-password 
Your Encryption Method:  your-encryption-method 

Your QR Code: (For Shadowsocks Windows, OSX, Android and iOS clients)
 ss://YWVzLTI1Ni1jZmI6ZHBjMTk5NHNwYXJrQDE3Mi45Ni4yMjQuMjEyOjg5ODk= 
Your QR Code has been saved as a PNG file path:
 /root/shadowsocsks/shadowsocks_libev_qr.png 

Welcome to visit: https://teddysun.com/486.html
Enjoy it!
```



#### 卸载方法

如果上述选择过程中按错或者想重新安装，使用root用户登录到脚本的位置运行：

```
./shadowsocks-all.sh uninstall
```



#### 参数方式启动脚本

安装完成之后默认shadowsocks-libev是开启启动的

启动脚本后面的参数含义，从左至右依次为：启动，停止，重启，查看状态。

| 版本               | 脚本位置                       | 启动/停止  | 重启    | 查看状态 |
| ------------------ | ------------------------------ | ---------- | ------- | -------- |
| Shadowsocks-Python | /etc/init.d/shadowsocks-python | start/stop | restart | status   |
| ShadowsocksR       | /etc/init.d/shadowsocks-r      | start/stop | restart | status   |
| Shadowsocks-Go     | /etc/init.d/shadowsocks-go     | start/stop | restart | status   |
| Shadowsocks-libev  | /etc/init.d/shadowsocks-libev  | start/stop | restart | status   |



#### 各个版本配置文件位置（默认）

| Shadowsocks-Python 版： | /etc/shadowsocks-python/config.json |
| ----------------------- | ----------------------------------- |
| ShadowsocksR 版：       | /etc/shadowsocks-r/config.json      |
| Shadowsocks-Go 版：     | /etc/shadowsocks-go/config.json     |
| Shadowsocks-libev 版：  | /etc/shadowsocks-libev/config.json  |



### 速度测试

![1545822424646](speed_bbr.png)

教育网情况下测试4K视频最快是35139kbps

![1545822491223](speed_ovz.png)

原先的openVZ VPS+finalSpeed加速 速度一直保持在35595kbps，稍微好于bbr加速版的ss

![手机测试](speed_mobile.jpg)

移动端测试一下,1080p也可以运行流畅，运营商是中国联通，相较于之前的VPS手机上的体验好了不少（ps：手机开了ss客户端之后发热严重，iphone7 plus）



### 总结

虽然没赶上黑五买到39.99的GIA，而且相对花费较之前的CN2贵了点，但是搬瓦工提供的内置bbr还是免去了很多麻烦，尤其是内和不兼容升级内核的问题，速度方面目前在学校的感觉还可以，之后再在其他网络环境下进行测试更新，总体来说安装一个shadowSocks服务端还是很方便的一件事，感谢大佬的一键安装脚本。