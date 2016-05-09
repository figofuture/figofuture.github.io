---
layout: post
title:  "做自己的科学上网，让别人羡慕去吧"
subtitle:   "科学上网是如何上网学科学的第一步，翻出去，看更大的世界，学更多的知识，了解更多的文化，开阔更广袤的技术视野。笔者总结和实践了一些自己搭建VPN的经验和经历，分享给大家。"
date:       2016-05-04
author:     "figotan"
header-img: "img/header/20160504.jpg"
header-mask: 0.5
catalog: true
tags:
    - vpn
    - pptp
    - l2tp
    - ipsec
    - vps
<<<<<<< HEAD
    - strongswan
    - ike
=======
    - ubuntu
>>>>>>> 3c7ab267c1945f9e1ae73bb78c5d8de2e0226dfd
    - 2016
---

首先讲为什么所有的事情都要自己做，当下市面上已经有了如此之多的解决方案和产品，开源的，免费的，收费的，各种各样的，因为某些原因这里就不一一列举了。但是除了全程参与客户端设置和服务器部署的开源解决方案之外，几乎所有的产品和解决方案都有一个值得用户质疑的问题：**安全**。或者说的更准确一些，那就是：**隐私安全**。  
如果你是**科学上网**的用户，你可以问问自己这样一个问题，你花钱或者不花钱用的某一种科学上网服务，它保证不会监听你的上网行为吗？  
所以我决定自己花钱买VPS，搭建一个专属自己的VPN服务器。

# 如何选择VPS
说起自费自搭自建VPN上网服务，第一步是如何选择购买VPS服务。  

> VPS（Virtual Private Server 虚拟专用服务器）技术，将一部服务器分割成多个虚拟专享服务器的服务。每个VPS都可分配独立公网IP地址、独立操作系统、独立空间、独立内存、独立CPU资源、独立执行程序和独立系统配置等。每一个VPS主机均可独立进行重启，并拥有自己的root访问权限、用户、IP地址、内存、进程、文件、应用程序、系统函数库以及配置文件。   

选择VPS比自己租用机房的硬件服务器的成本要低。关于VPS的详细介绍可以参考[维基百科](https://en.wikipedia.org/wiki/Virtual_private_server)。  
国内和国外有很多VPS的供应商，出于某些需求，我选的是国外的VPS，选择国外的VPS服务主要考虑三个方面的问题，

* 服务商的口碑
* 访问速度
* 性价比

#### 服务商的口碑
**服务商的口碑**是个很难衡量的问题，其他两个问题其实也和这个问题有直接或者间接的联系。关于口碑，一般是去一些靠谱的VPS推荐网站看看别人的评价。比如国内比较知名靠谱的[**熠锋美国主机评论**](http://www.yfushost.com/)，或者去quora，知乎（知乎上目前一般会推荐给你[Linode](https://www.linode.com), [DigitalOcean](https://www.digitalocean.com)和[Vultr](https://www.vultr.com)）上找找答案。  

#### 访问速度
关于**访问速度**，如果服务商提供试用可以自己评测，或者去前面提到的网站上找找别人的评测文章，另外一个需要注意的是机房的选择，如果选择美国的主机，机房最好选在西海岸的一些城市，比如洛杉矶之类的，因为中国大陆相较于美国的城市中，西海岸是最近的，如果你在欧洲，那选东海岸的吧。如果有台湾，香港，日本和新加坡的机房那就最好不过了，不过就算有，貌似价格贵不少的，所以到了最后一个问题。  

#### 性价比
**性价比**这个要自己看，选择几核心的CPU，多大的内存，多少容量的磁盘，多大的带宽，几个独立的IP地址，虚拟方式([OpenVZ](https://openvz.org/Main_Page), [Xen](http://www.xenproject.org), [KVM](http://www.linux-kvm.org/page/Main_Page))等，最后这些性能都会映射到价格。一分钱一分货，我的建议是合适就好，土豪除外。服务商一般会提供一些套餐组合，以及某段时间放出的特惠套餐，每年在圣诞节前后还会有一些优惠套餐的推出以及优惠码的放送。[**熠锋美国主机评论**](http://www.yfushost.com/)这个网站时常有收集最近的一些优惠信息，如果想找可以去看看。  

#### 如何测试VPS性能
一些测试VPS性能的脚本工具可供参考：

* [vpsbench](https://github.com/mgutz/vpsbench)
* [Freevps](https://freevps.us/downloads/bench.sh)
* [一键测试脚本bench.sh](https://teddysun.com/444.html)

# 选择操作系统
再来是主机操作系统的选择，一般廉价的VPS都只提供Linux操作系统，我目前只有购买过安装Linux的VPS主机，所以想看Windows的可以就此打住了。就算是Linux操作系统，也只有提供某几个发行版的选择，[Ubuntu](http://www.ubuntu.com/global), [CentOS](https://www.centos.org), [Debian](https://www.debian.org/), [Arch Linux](https://www.archlinux.org/)是常客。对于Linux操作系统，我个人除了[Gentoo](https://gentoo.org/)和[LFS](http://linuxfromscratch.org/)以外，基本上不太常用其他的发行版。而支持Gentoo的VPS供应商(**Linode**)非常少，LFS几乎就没有了，至少还没有找到过。我购买的第一个VPS主机服务是[**Chicago VPS**](https://www.chicagovps.net)提供的年付12美刀基于Gentoo 64位的VPS,当时购买只是因为廉价和Gentoo，但是配置超烂，而且速度狂慢无比，所以在上面也没有做什么东西，基本上是为了以前给公司的一个创新项目做demo用了一下，后来废弃掉再也没有使用了。  
再后来入手一个VPS（已被**Chicago VPS**收购），看起来配置还可以，性价比较高，配置是

```
2048MB RAM 
4 CPU Cores  
40GB DiskSpace 
2500GB Bandwidth 
1 IPv4 Address 
OpenVZ/SolusVM
```

价格年付只要$30，平均每月$2.5，访问速度还可以，主要是年付30刀这样的价格应该算蛮便宜了。已经在上面成功运行http/https的网络代理服务**tinyproxy**和基于**PPTP**的VPN，手机上访问G+和Youtube比较流畅。唯一遗憾是不支持Gentoo系统，我选择了安装Ubuntu系统。想要知道更多的Linux发行版情况，请参考[DistroWatch](http://distrowatch.com/)  
非Linux系统，除了Windows外，UNIX世界里最流行的估计是[**FreeBSD**](http://www.freebsd.org/)了，它是**Gentoo**的灵感来源。  

以上提到的Linux，除了Gentoo和LFS适合Linux系统的开发者外，其他都适合用来做服务端部署和网站运营。个人觉得Ubuntu和CentOS是目前服务器市场份额较大且资料最全的两个选择了。

# 从VPS到VPN
因为我租用的VPS是基于OpenVZ虚拟技术的。在网上查了下，有说只有基于Xen虚拟技术的VPS才可以做VPN，其实也不尽然。后来看到[一篇帖子总结了什么样的VPS才可以做VPN服务](http://www.worlduc.com/blog2012.aspx?bid=12615260)，摘录如下：  
OpenVZ一般可以搭建OpenVPN（只需开启Tun模块）有些还能搭建PPTP VPN和L2TP VPN（纯L2TP没有IPSEC加密）（只需开启PPP模块）（另外都需开启iptable nat模块以便转发上网），Xen/KVM的VPS可以搭建各种常见类型VPN。

#### OpenVZ（不开Tun/Tap模块）
有些OpenVZ的VPS服务商连Tun/Tap模块都不给开更别说PPP模块了（比如Host1Free的免费VPS），这样基本上就没办法直接搭建VPN服务了。

#### OpenVZ（开Tun/Tap模块）
一般收费VPS服务商（比如123systems）都会开Tun/Tap和iptable nat模块，有些VPS服务商的Tun/Tap模块默认开启，有的可以在面板后台自己打开，有的需要TK客服帮助打开，一般开Tun/Tap模块的服务商都会同时开启开iptable nat模块，这样就可以搭建OpenVPN服务了。
登录VPS后输入命令

```
cat /dev/net/tun
```

如果返回
  
> cat: /dev/net/tun: File descriptor in bad state

就说明tun模块已经开启  
输入命令  

```
iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o venet0 -j MASQUERADE
```

如果返回

> iptables: No chain/target/match by that name
 
就说明nat模块正常（也不一定都是返回这个结果，只要**不提示iptables或nat不存在就行**）

#### OpenVZ（开PPP模块）
不少VPS服务商除了可以开Tun/Tap和iptable nat模块外还可以开PPP模块（比如buyvm），这样不但可以搭建OpenVPN还可以搭建PPTP VPN和L2TP VPN（纯）了，PPP模块有的服务商默认开启，有的面板可自行开启，有的需要TK客服开启或编译。  
登录VPS后输入命令  

```
cat /dev/ppp
```

如果返回

> cat: /dev/ppp: No such device or address

就说明ppp模块启动了（但是并不一定能用，如果不能用还是要TK客服开启相关设置的）

#### Xen/KVM
Xen/KVM类型的VPS基本上可以搭建所有常见类型的VPN，比如OpenVPN、PPTP VPN、L2TP VPN（纯）、L2TP IPSec VPN、IPSec VPN（兼容Cisco或Windows 7及更高版本系统适用的IKEv2或适用塞班系统mVPN客户端）、SSTP VPN（需要VPS上安装Windows2008及更高版本）  

#### 总结
在VPS上搭建VPN，对于虚拟方式的考虑，直接关系到VPN协议的支持与否，所以总结在这里

* OpenVPN最容易实现也被最多VPS服务商支持
* 其次是PPTP/L2TP
* OpenVZ貌似还不能虚拟IPSec所以目前都不支持L2TP IPSec VPN、Cisco IPsec VPN、IKEv2 IPSec VPN
* Xen/KVM都比较强大，常见VPN都能搭建，所以是最好的选择，可惜价格一般比OpenVZ贵了不少

# PPTP
下面就介绍下如何在VPS上搭建VPN的服务，从PPTP开始。
其实我在网上找了一堆资料，失败过N次后爬起来再折腾，最后终于成功。失败的实验就不讲了，这里讲讲整理过后的流程。

#### 开启ppp
首先ssh登录到申请的VPS机器上，检测PPP是否开启，命令如下

```
cat /dev/ppp
```
开启成功的标志：
> cat: /dev/ppp: No such file or directory 或者 cat: /dev/ppp: No such device or address

可以继续安装过程；  
开启不成功的标志：
> cat: /dev/ppp: Permission denied

登录到VPS供应商提供的web portal端去自行"Enable PPP"，如果没有提供这样的开关的话那就只能给VPS提供商Submit一个Ticket请求开通了：

```
Hello
  Could you enabled PPP for me? I want run pptp-vpn on my VPS.
Thank you.
```
成功开启PPP以后，就可以安装了。有些教程里说还需要输入cat /dev/net/tun检测tun/tap是否开启，实际上这是完全不必要的。只是搭建PPTP，不需要TUN/TAP模块。

#### 安装必要软件
安装ppp和iptables

```
sudo apt-get install ppp pptpd iptables iptables-persistent
```

#### 配置pptp
编辑**/etc/pptpd.conf**文件，把下面字段前面的#去掉即可，此处的IP段可以任意，但是尽量注意不要跟本地网络的IP段有冲突。

```
localip 192.168.0.1
remoteip 192.168.0.234-238,192.168.0.245
```

#### 配置ppp
编辑**/etc/ppp/pptpd-options**文件，去掉ms-dns前面的#，并修改成如下字段：

```
ms-dns 8.8.8.8
ms-dns 8.8.4.4
```

设置PPTP VPN密码，修改**/etc/ppp/chap-secrets**文件，添加一行，内容如下（空格隔开）：

```
用户名    pptpd    密码    *
```
第二行pptpd表示服务名称，在/etc/ppp/pptpd-options里**name**配置项中指定  
最后一行"*"表示任何客户端ip都允许

#### ip转发
修改内核设置，使其支持IP转发,编辑/etc/sysctl.conf文件，去掉"net.ipv4.ip_forward"左边的“#“：

```
net.ipv4.ip_forward=1
```

如下命令让配置生效

```
sudo sysctl -p
```

#### iptables设置
添加iptables转发规则，并保存

```
sudo iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -j SNAT --to-source ***.***.***.*** 
sudo iptables -A FORWARD -s 192.168.0.0/24 -p tcp -m tcp --tcp-flags 
FIN,SYN,RST,ACK SYN -j TCPMSS --set-mss 1356

```
或者这样

```
sudo iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -j MASQUERADE -o eth0 
```

注意

* 192.168.0.0/24为**/etc/pptpd.conf**里分配的remoteip的地址范围
* 转发规则可以用VPS的ip地址，也可以用VPS的对外网络接口， 分别是 ```-j SNAT --to-source 111.111.111.111```(111.111.111.111为你VPS的公网IP地址) 或者 ```-j MASQUERADE -o eth0```(eth0为你VPS的对外网络接口，可以用命令**ifconfig**查看)

持久化保存iptables的规则

```
sudo service iptables-persistent save
```

#### 服务生效
重启pptpd服务

```
sudo service pptpd restart
```

# L2TP
因为Android客户端已经不再支持非IPSEC加密方式的VPN配置了，所以这里只说L2TP on IPSEC的搭建方式

#### 安装软件

```
sudo apt-get install strongswan strongswan-plugin-xauth-generic ppp xl2tpd
```

#### 配置IPSEC
编辑文件**/etc/ipsec.conf**, 输入如下内容

```
conn vpnserver
        type=transport
        authby=secret
        pfs=no
        rekey=no
        keyingtries=1
        left=%any
        leftprotoport=udp/l2tp
        right=%any
        rightprotoport=udp/%any
        auto=add
```

编辑文件**/etc/ipsec.secrets**，添加认证方式

```
#验证用户所需的信息
: PSK "SECRET" # 这里 SECRET 可随意替换成你想要的密钥
```
> 这里的**SECRET**就是iOS客户端配置里的"**Secret**"或者Android客户端配置里的"**IPSec pre-shared key**"


#### 配置xl2tpd
编辑文件**/etc/xl2tpd/xl2tpd.conf**，输入如下内容

```
[global]
ipsec saref = yes
saref refinfo = 30
port = 1701
access control = no
;debug avp = yes
;debug network = yes
;debug state = yes
;debug tunnel = yes

[lns default]
ip range = 172.16.1.30-172.16.1.100
local ip = 172.16.1.1
refuse pap = yes
require authentication = yes
;ppp debug = yes
pppoptfile = /etc/ppp/options.xl2tpd
length bit = yes
```

#### 配置ppp
编辑文件**/etc/ppp/options.xl2tpd**，输入如下内容

```
require-mschap-v2
ms-dns 8.8.8.8
ms-dns 8.8.4.4
auth
mtu 1200
mru 1000
crtscts
hide-password
modem
name l2tpd
proxyarp
lcp-echo-interval 30
lcp-echo-failure 4
noccp
nodefaultroute
lock
silent
```

设置PPTP VPN密码，修改**/etc/ppp/chap-secrets**文件，添加一行，内容如下（空格隔开）：

```
用户名    l2tpd    密码    *
```

第二行l2tpd表示服务名称，在**/etc/ppp/options.xl2tpd**里**name**配置项中指定

#### ip转发以及iptables配置
配置内容可参考pptp的相关配置，iptables转发ip地址根据文件**/etc/xl2tpd/xl2tpd.conf**中**ip range**配置的ip地址范围来设置

```
sudo iptables -t nat -A POSTROUTING -s 172.16.1.0/24 -j MASQUERADE -o eth0 
```

#### 服务生效
重启服务

```
sudo service strongswan restart
sudo service xl2tpd restart
```

启动或者重启xl2tpd服务的时候是不是会遇到
> setsockopt recvref[30]: Protocol not available xl2tpd.

甭管它，继续就可以了。

# IPSEC
是不是觉得L2TP on IPSEC的配置过于繁琐？那么去掉ppp的配置，直接上IPSEC如何？关于什么样的VPS支持IPSEC，可以参考前面的内容。

#### 安装软件

```
sudo apt-get install strongswan strongswan-plugin-xauth-generic
```

#### IPSEC配置
编辑文件**/etc/ipsec.conf**，实例配置如下

```
config setup
	uniqueids=no

conn ipsec_xauth_psk
	keyexchange=ikev1
	authby=xauthpsk
	xauth=server
	left=%defaultroute
	leftsubnet=0.0.0.0/0
	right=%any
	rightsubnet=10.0.0.0/24
	rightsourceip=10.0.0.0/24
	rightdns=8.8.8.8
	auto=add
```

编辑文件**/etc/ipsec.secrets**，添加认证方式

```
#验证用户所需的信息
: PSK "SECRET" # 这里 SECRET 可随意替换成你想要的密钥
你的用户名 : XAUTH "你的密码"
```
> 这里的**SECRET**就是iOS客户端配置里的"**Secret**"或者Android客户端配置里的"**IPSec pre-shared key**"

#### ip转发以及iptables配置
配置内容可参考pptp的相关配置，iptables转发ip地址根据文件**/etc/ipsec.conf**中    **rightsourceip**配置的ip地址范围来设置

```
sudo iptables -t nat -A POSTROUTING -s 10.0.0.0/24 -j MASQUERADE -o eth0 
```

#### 服务生效
重启服务

```
sudo service strongswan restart
```

# IKEv2
因特网密钥交换协议第二版

#### 安装软件
安装EAP

```
sudo apt-get install strongswan-plugin-xauth-eap strongswan-plugin-eap-mschapv2
```

#### IPSEC配置
编辑文件**/etc/ipsec.conf**，实例配置如下

```
config setup
	uniqueids=no

conn ipsec_ikev2_eap_psk
	keyexchange=ikev2
	left=%defaultroute
	leftsubnet=0.0.0.0/0
	leftauth=psk
	leftid=vpn.example.server
	right=%any
	rightsubnet=10.11.1.0/24
	rightsourceip=10.11.1.0/24
	rightdns=8.8.8.8
	rightauth=eap-mschapv2
	rightsendcert=never
	rightid=vpn.example.client
	eap_identity=%any
	auto=add
```

编辑文件**/etc/ipsec.secrets**，加入EAP认证用户名和密码

```
: PSK "SECRET" # 这里 SECRET 可随意替换成你想要的密钥
用户名 : EAP "密码"
```

#### ip转发以及iptables配置
**设置iptables地址转发，具体请参考PPTP/L2TP/IPSEC等**

#### IKEv2客户端配置(MAC&iPhone)
MAC OS X/iOS 客户端配置，虽然MAC OS X EI Capitan(10.11.4)和iOS 9的系统设置中可以手动添加IKEv2配置了，但是没法输入PSK(共享密钥)，所以只能用描述文件的方式了。

1. 在MAC的App Store中搜索并安装**Apple Configurator 2**
2. 启动**Apple Configurator 2**，点击"文件"->"新建描述文件"
3. 选择"VPN",然后点击"配置"
4. 输入"**连接名称**"，"**连接类型**"选择"**IKEv2**",“**服务器**”输入VPN服务器IP地址，“**远程标识符**”输入文件**/etc/ipsec.conf**中的"**leftid**"的值，“**局部标识符**”输入文件**/etc/ipsec.conf**中的"**rightid**"的值，"**设备鉴定**"请选择"**共享密钥**"，"**共享密钥**"请输入文件**/etc/ipsec.secrets**中的"**SECRET**"，勾上“**启用 EAP**”，“**EAP 鉴定**”选择“**用户名/密码**”，“**账户**”和“**密码**”是文件**/etc/ipsec.secrets**中的"**用户名**"和"**密码**"(这里必须填写，不然保存的描述文件中的“**EAP 鉴定**”会变成"**证书**"，从而导致连接VPN失败)，具体内容可以参考下图所示
![](http://www.figotan.org/img/in-post/Snip20160504_5.png)  
5. 在设备上安装描述文件，这样设备上就可以使用这个VPN了。安装方式，如果是安装到MAC上，双击描述文件即可安装；iPhone则可以通过**Apple Configurator 2**来安装，用USB将iPhone连接到MAC，然后打开**Apple Configurator 2**安装描述文件；如果没有MAC，可以将描述文件部署到Web Server上，然后iPhone上打开Safari，输入描述文件完整的URL地址，完成安装。

# 证书访问方式
一般客户端访问VPN服务器的方式不外乎需要这三样东西：用户名/密码/共享密钥。  
恩，说起用户名／密码确实是一种很头疼的认证方式，天杀的用户名／密码，要记那么多，各种问题，密码泄漏了咋办？  
安全和记忆是用户名／密码永远的抹不去的伤痛。  
那么，如何解决呢？忘掉用户名／密码吧，证书来了。

#### 证书是啥？
数字证书就是互联网通讯中标志通讯各方身份信息的一串数字，提供了一种在Internet上验证通信实体身份的方式，数字证书不是数字身份证，而是身份认证机构盖在数字身份证上的一个章或印（或者说加在数字身份证上的一个签名）。它是由权威机构——CA机构，又称为证书授权（Certificate Authority）中心发行的，人们可以在网上用它来识别对方的身份。  
数字证书有两大类：

* 签名证书：由权威颁发机构(CA)颁发给服务器或者个人用于证明自己身份的东西，默认客户端都是信任的。主要目的是用来加密和保证数据的完整性和不可抵赖性。
* 自签名证书：由服务器自己颁发给自己，用于证明自己身份的东西（签名实体发布给自身的证书,即发布者和证书主体相同），非权威颁发机构发布，默认客户端都是不信任的，主要目的是用来加密和保证数据的完整性和不可抵赖性,与签名证书相同。

最大的区别是签名证书是请大家公认的权威机构签发，且所有客户端都认同这个证书；而自签名证书是自己签发给自己的，其他客户端都不会认同这个证书（客户端浏览器会提示用户手工确认是否认同这个证书）。  

<<<<<<< HEAD
# 签名证书方式
签名证书可以花钱购买，也可以采用Let's Encrypt方案，具体可以参考这两篇博客：[用 Let‘s Encrypt 证书搭建 IKEv2 VPN](http://blog.zorro.im/posts/strongswan-ikev2-for-ios-with-letsencrypt.html)和[Strongswan on Ubuntu 16.04 for iOS 9 Client](http://dcamero.azurewebsites.net/strongswan-ubuntu-1604-ios-9.html)  

#### 使用Let's Encrypt提供的免费签名证书
关于Let's Encrypt，这里不多说，详细内容可以参考我之前的博客文章介绍。因为[Let's Encrypt目前不支持给IP地址做签名](https://community.letsencrypt.org/t/certificate-for-public-ip-without-domain-name/6082)，所以这种方案的前提是你必须先申请一个域名。如果没有域名，请跳过这种方式，使用**自签名证书方式**。

登录到你的VPS，从Github签出[Let’s Encrypt](https://github.com/letsencrypt/letsencrypt)的源代码

```bash
git clone https://github.com/letsencrypt/letsencrypt
```

进入本地源代码目录

```bash
cd letsencrypt
```

Let’s Encrypt提供多种认证方式，因为之前在VPS上有了HTTP的网站，所以这里采用了**webroot**的方式，其他方式请参考[官方文档](https://letsencrypt.readthedocs.org/en/latest/using.html#plugins)  
如果是**主域名**的认证：

```bash
./letsencrypt-auto --debug certonly --webroot --email ${NAME}@${VPN_SERVER_DOMAIN} -d www.${VPN_SERVER_DOMAIN} -d {VPN_SERVER_DOMAIN} -w /var/www/{VPN_SERVER_DOMAIN}
```

**子域名**的认证：

```bash
./letsencrypt-auto --debug certonly --webroot --email ${NAME}@${VPN_SERVER_DOMAIN} -d ${SUB_DOMAIN}.${VPN_SERVER_DOMAIN} -w /var/www/${VPN_SERVER_DOMAIN}/${SUB_DOMAIN}
```

然后在弹出的蓝底白字提示框中一路点击"OK"

注意如下问题：

* 请将命令中的**${NAME}**, **${VPN_SERVER_DOMAIN}**, **${SUB_DOMAIN}**替换成你自己的名字，域名以及子域名  
* 因为Gentoo目前是在试验阶段，所以命令行加上**--debug**参数
* 参数**--email**如果没有在命令行加上，会在随后弹出的对话框里提示你填写
* **-w**指定Web服务器网址内容放置的目录，请指定自己放置的目录

生成的证书放在**/etc/letsencrypt/live/[网站域名]**下  

| 文件名     | 内容             | 
|:-------------|:----------------------------------|
| cert.pem      | 服务端证书       |
| chain.pem     | 浏览器需要的所有证书但不包括服务端证书，比如根证书和中间证书           |
| fullchain.pem | 包括了cert.pem和chain.pem的内容 |
| privkey.pem   | 证书的私钥|

一般情况下**fullchain.pem**和**privkey.pem**就够用了，VPN服务器配置也只用到了这两个文件。因为**IPSEC**读取证书和私钥的路径和用Let’s Encrypt生成的文件的路径不同，所以需要做如下软链接（软链接的优势是当证书自动升级后，软链接可以不变）

```
sudo ln -s /etc/letsencrypt/live/www.${VPN_SERVER_DOMAIN}/fullchain.pem /etc/ipsec.d/certs/fullchain.pem
sudo ln -s /etc/letsencrypt/live/www.${VPN_SERVER_DOMAIN}/privkey.pem /etc/ipsec.d/private/privkey.pem
```

还有个坑是权限问题，在strongswan启动的时候，加载**privkey.pem**和**fullchain.pem**，**fullchain.pem**会报没有权限，而**privkey.pem**则没有这个问题。如何解决？修改访问权限，貌似没用，那么把**/etc/letsencrypt/live/www.${VPN_SERVER_DOMAIN}/fullchain.pem**拷贝到**/etc/ipsec.d/certs/fullchain.pem**吧，证书更新麻烦些了

#### 证书的更新
Let’s Encrypt生成的证书有效期是90天（三个月），所以开启一个定时任务（cron）自动做更新
添加为定时任务, 编辑这个文件

```bash
sudo vi /etc/cron.monthly/letsencrypt_renew
```

添加如下内容：

```bash
#!/bin/sh
/path/to/letsencrypt/letsencrypt-auto --debug renew > /var/log/letsencrypt/renew.log 2>&1
```

注意 **/path/to/letsencrypt**是你的Let’s Encrypt工具的安装全路径，这个脚本可以做一个更新后证书的拷贝动作，自动将更新的证书拷贝到**IPSEC**能识别的目录中，在上面那条命令后添加如下内容

```
sudo cp /etc/letsencrypt/live/www.${VPN_SERVER_DOMAIN}/fullchain.pem /etc/ipsec.d/certs/fullchain.pem
```

授予**/etc/cron.monthly/letsencrypt_renew**可执行权限

```bash
sudo chmod a+x /etc/cron.monthly/letsencrypt_renew
```

#### 配置IPSEC
编译文件**/etc/ipsec.conf**，内容为

``` ipsec
conn IPSec_ikev2_rsa
    keyexchange=ikev2
    leftid=${VPN_SERVER_DOMAIN}
    rightauth=eap-mschapv2
    eap_identity=%any

conn %default
    keyexchange=ikev2
    dpdaction=hold
    dpddelay=600s
    dpdtimeout=5s
    lifetime=24h
    ikelifetime=240h
    rekey=no
    left=%any
    leftsubnet=0.0.0.0/0
    leftcert=fullchain.pem
    leftsendcert=always
    right=%any
    rightdns=8.8.8.8
    rightsourceip=10.0.0.0/24
    auto=add
```

注意 **${VPN_SERVER_DOMAIN}**一定要和用Let’s Encrypt生成证书的域名一致

编辑文件**/etc/ipsec.secrets**，增加

```
 : RSA privkey.pem
 用户名 : EAP "密码"
```

#### ip转发以及iptables配置
**设置iptables地址转发，具体请参考PPTP/L2TP/IPSEC等**

#### 客户端配置
如果是iOS，将Let’s Encrypt根证书安装到手机，手机访问https://letsencrypt.org/certificates/，点击**PEM**格式的**Let’s Encrypt Authority X1**证书并安装。  
打开系统设置，添加VPN，类型选择"IKEv2"，服务器地址和远程ID为**${VPN_SERVER_DOMAIN}**，本地ID可以不填，验证方式选择用户名／密码，具体内容是**/etc/ipsec.secrets**里的用户名和密码。


# 自签名证书方式
签名证书需要有域名，如果VPN服务器还没有绑定域名，那么，如果用证书的方式，只有采用自签名证书啦。这篇文章[如何在 VPS 上搭建 VPN 来翻墙](http://www.jianshu.com/p/2f51144c35c9)里提到了用文章[iOS8 不越狱翻墙方案](https://songchenwen.com/tech/2014/10/13/cross-fire-wall-on-ios8/)的脚本来创建服务端和客户端的证书，这篇提到了参考[Setup IKEv2 On Demand VPN on iOS 8 and IKEv2, IKEv1 Cisco IPSec VPN with Strongswan](https://maoxian.de/2014/10/1220.html)的内容，同时，这篇文章[IPSEC VPN on Ubuntu 15.04 with StrongSwan](https://raymii.org/s/tutorials/IPSEC_vpn_with_Ubuntu_15.04.html#Certificates)在证书的生成部分参考了[strongSwan 5: How to create your own private VPN](https://www.zeitgeist.se/2013/11/22/strongswan-howto-create-your-own-vpn/)  

=======
签名证书可以花钱购买，也可以采用Let's Encrypt方案，具体可以参考这两篇博客：[用 Let‘s Encrypt 证书搭建 IKEv2 VPN](http://blog.zorro.im/posts/strongswan-ikev2-for-ios-with-letsencrypt.html)和[Strongswan on Ubuntu 16.04 for iOS 9 Client](http://dcamero.azurewebsites.net/strongswan-ubuntu-1604-ios-9.html)  
签名证书需要有域名，如果VPN服务器还没有绑定域名，那么，如果用证书的方式，只有采用自签名证书啦。这篇文章[如何在 VPS 上搭建 VPN 来翻墙](http://www.jianshu.com/p/2f51144c35c9)里提到了用文章[iOS8 不越狱翻墙方案](https://songchenwen.com/tech/2014/10/13/cross-fire-wall-on-ios8/)的脚本来创建服务端和客户端的证书，这篇提到了参考[Setup IKEv2 On Demand VPN on iOS 8 and IKEv2, IKEv1 Cisco IPSec VPN with Strongswan](https://maoxian.de/2014/10/1220.html)的内容，同时，这篇文章[IPSEC VPN on Ubuntu 15.04 with StrongSwan](https://raymii.org/s/tutorials/IPSEC_vpn_with_Ubuntu_15.04.html#Certificates)在证书的生成部分参考了[strongSwan 5: How to create your own private VPN](https://www.zeitgeist.se/2013/11/22/strongswan-howto-create-your-own-vpn/)  


>>>>>>> 3c7ab267c1945f9e1ae73bb78c5d8de2e0226dfd
#### 生成自签名证书
现在梳理下自签名证书的生成过程。  

生成自签名CA根证书

```
sudo ipsec pki --gen --type rsa --size 4096 --outform pem > /etc/ipsec.d/private/strongswanKey.pem
sudo chmod 600 /etc/ipsec.d/private/strongswanKey.pem
sudo ipsec pki --self --lifetime 3650 \
	--in /etc/ipsec.d/private/strongswanKey.pem --ca --type rsa \
	--dn "C=CH, O=strongSwan, CN=strongSwan Root CA" \
	--flag serverAuth --outform pem \
	> /etc/ipsec.d/cacerts/strongswanCert.pem
```

注意

* --lifetime 3650表示证书有效期是10年，这里可以自己改，按天计
* -–flag serverAuth 避免Windows Phone 8.1客户端连接失败

打印自签名CA根证书内容

```
sudo ipsec pki --print --in /etc/ipsec.d/cacerts/strongswanCert.pem
```

生成服务端证书

```
sudo ipsec pki --gen --type rsa --size 2048 --outform pem > /etc/ipsec.d/private/vpnHostKey.pem
sudo chmod 600 /etc/ipsec.d/private/vpnHostKey.pem
sudo ipsec pki --pub --in /etc/ipsec.d/private/vpnHostKey.pem --type rsa | \
	sudo ipsec pki --issue --lifetime 730 \
	--cacert /etc/ipsec.d/cacerts/strongswanCert.pem \
	--cakey /etc/ipsec.d/private/strongswanKey.pem \
	--dn "C=CH, O=strongSwan, CN=${VPN_HOST}" \
	--san ${VPN_HOST} \
	--flag serverAuth --flag ikeIntermediate \
	--outform pem > /etc/ipsec.d/certs/vpnHostCert.pem
```

注意事项

* --lifetime 730表示有效期是2年，这里可以自己指定时间，按天计
* ${VPN_HOST}为你的VPN服务器域名或者IP地址，--dn(subject Distinguished Name)的CN和--san(subject Alternative Name)都需要指定一致，否则客户端会连接不上VPN服务器。
* --flag serverAuth Windows客户端需要
* --flag ikeIntermediate OS X 10.7.3及以下客户端系统需要

打印服务端证书内容

```
sudo ipsec pki --print --in /etc/ipsec.d/certs/vpnHostCert.pem
```

生成客户端证书

```
sudo ipsec pki --gen --type rsa --size 2048 --outform pem > /etc/ipsec.d/private/${USER_NAME}Key.pem
sudo chmod 600 /etc/ipsec.d/private/${USER_NAME}Key.pem
sudo ipsec pki --pub --in /etc/ipsec.d/private/${USER_NAME}Key.pem --type rsa | \
	sudo ipsec pki --issue --lifetime 730 \
	--cacert /etc/ipsec.d/cacerts/strongswanCert.pem \
	--cakey /etc/ipsec.d/private/strongswanKey.pem \
	--dn "C=CH, O=strongSwan, CN=${USER_EMAIL}" \
	--san ${USER_EMAIL} \
	--outform pem > /etc/ipsec.d/certs/${USER_NAME}Cert.pem
```

注意事项

* --lifetime 730表示有效期是2年，这里可以自己指定时间，按天计
* ${USER_EMAIL}为你的电子邮箱地址，--dn(subject Distinguished Name)的CN和--san(subject Alternative Name)都需要指定一致，否则客户端会连接不上VPN服务器。
* ${USER_NAME}请指定为自己的用户名

打印客户端证书内容

```
sudo ipsec pki --print --in /etc/ipsec.d/certs/${USER_NAME}Cert.pem
```

将客户端证书和客户端私钥导出为PKCS#12格式文件

```
sudo openssl pkcs12 -export -inkey /etc/ipsec.d/private/${USER_NAME}Key.pem \
	-in /etc/ipsec.d/certs/${USER_NAME}Cert.pem -name "${USER_NAME}'s VPN Certificate" \
	-certfile /etc/ipsec.d/cacerts/strongswanCert.pem \
	-caname "strongSwan Root CA" \
	-out /etc/ipsec.d/${USER_NAME}.p12
```

私钥和它的证书文件通常是两个不同的文件。有时候同时需要私钥和证书会显得不方便，所以就有了**PKCS#12**文件，它将私钥和证书保存在同一个文件中，并且由一个在产生的时候用户输入的密码来保护。

将**/etc/ipsec.d/cacerts/strongswanCert.pem**和**/etc/ipsec.d/${USER_NAME}.p12**这两个文件下载到本地，这两个文件提供给客户端使用（后面会讲如何使用）。下载的方法有很多中，最常见的是**scp**和**rsync**（当然也可以用sftp/ftp，web方式等等），scp命令参考如下，请在本地使用

```
scp username@[VPS_ADDRESS]:/etc/ipsec.d/cacerts/strongswanCert.pem .
scp username@[VPS_ADDRESS]:/etc/ipsec.d/${USER_NAME}.p12 .
```

username是远程VPS主机分配的用户名  
VPS_ADDRESS是远程VPS主机的域名或者IP地址

#### 生成的文件清单
到目前为止，上述证书的生成过程中产生了如下文件：

|文件|用途|
|:-------|:--------:|
|/etc/ipsec.d/private/strongswanKey.pem|自签名CA私钥|
| /etc/ipsec.d/cacerts/strongswanCert.pem |自签名CA证书|
| /etc/ipsec.d/private/vpnHostKey.pem |服务端私钥|
|/etc/ipsec.d/certs/vpnHostCert.pem|服务端证书|
|/etc/ipsec.d/private/${USER_NAME}Key.pem|客户端私钥|
|/etc/ipsec.d/certs/${USER_NAME}Cert.pem|客户端证书|
|/etc/ipsec.d/${USER_NAME}.p12|客户端PKCS#12文件|

#### 题外话，关于客户端证书的撤销（撤回，回收）
如果客户端证书丢失或者被盗（泄漏），为了保证你的VPN服务不会被滥用，所以需要让这个证书失去作用，一个是等着它过期（太笨啦），另一个就是用下面的办法撤销证书。

如果是第一次撤销证书

```
sudo ipsec pki --signcrl --reason key-compromise \
	--cacert /etc/ipsec.d/cacerts/strongswanCert.pem \
	--cakey /etc/ipsec.d/private/strongswanKey.pem \
	--cert /etc/ipsec.d/certs/${USER_NAME}Cert.pem \
	--outform pem > /etc/ipsec.d/crls/crl.pem
```

这样被撤销的证书会被加入到一个撤销证书的列表中crl.pem(Certificate Revocation List)

继续增加被撤销的证书

```
sudo cp /etc/ipsec.d/crls/crl.pem /etc/ipsec.d/crl.pem.tmp
sudo ipsec pki --signcrl --reason key-compromise \
	--cacert /etc/ipsec.d/cacerts/strongswanCert.pem \
	--cakey /etc/ipsec.d/private/strongswanKey.pem \
	--cert /etc/ipsec.d/certs/${AnotherStolen}Cert.pem \
	--lastcrl /etc/ipsec.d/crl.pem.tmp \
	--outform pem > /etc/ipsec.d/crls/crl.pem
sudo rm /etc/ipsec.d/crl.pem.tmp
```

注意事项

* ${USER_NAME}Cert.pem和${AnotherStolen}Cert.pem请改成相应的证书名。
* 自签名CA证书私钥**/etc/ipsec.d/private/strongswanKey.pem**请妥善保存到安全的地方，非互联网能触及的地方，防止被盗。

#### IPSEC配置
因为目前ikev2的证书访问方式还没有搞定，所以先说说ikev1的配置  
编辑文件**/etc/ipsec.conf**，实例配置如下

```
conn ipsec_rsa
        keyexchange=ikev1
        left=%any
        leftsubnet=0.0.0.0/0
        leftcert=vpnHostCert.pem
        leftsendcert=always
        right=%any
        rightdns=8.8.8.8
        rightsourceip=10.0.0.0/8
        rightauth=pubkey
        rightauth2=xauth
        auto=add
```

注意**leftid**的内容**${VPN_HOST}**和生成服务端证书时CN与--san的内容保持一致

编辑文件**/etc/ipsec.secrets**，加入证书验证方式

```
: RSA vpnHostKey.pem
用户名 : EAP "密码"
```

#### ip转发以及iptables配置
**设置iptables地址转发，具体请参考PPTP/L2TP/IPSEC等**

#### 客户端配置
如果是MAC/iPhone客户端，有两种方式：

1. 将自签名CA根证书(**/etc/ipsec.d/cacerts/strongswanCert.pem**)和客户端PKCS#12文件(**/etc/ipsec.d/${USER_NAME}.p12**)下载到本地，并安装到设备MAC/iOS中，具体安装方式：对于MAC来说，将文件下载到本地，然后双击启动"钥匙串访问"即可安装;对于iPhone，需要将自签名CA根证书和客户端PKCS#12文件通过邮件发送到iPhone打开安装，或者部署到Web Server,然后手机通过Safari访问URL安装。安装完成后，在系统设置的地方配置VPN。是不是太繁琐了？
2. 生成VPN连接的描述文件，安装描述文件到设备中。将自签名CA根证书和客户端PKCS#12文件下载到本地MAC上，然后制作描述文件。关于描述文件如何安装，请查阅前面的叙述。

注意，证书在IPSEC中是用来取代共享密钥的，所以用户名密码的认证（XAUTH或者EAP的方式）仍然需要。

**最近笔者购买了DigitalOcean的VPS，所有上述方案，均本人亲测可行！！！如果有问题，可留言探讨解答相关技术，仅限技术讨论，其他免谈！！！**

# 分流与加速
可以参考[iOS8 不越狱翻墙方案](https://songchenwen.com/tech/2014/10/13/cross-fire-wall-on-ios8/)和[如何在 VPS 上搭建 VPN 来翻墙](http://www.jianshu.com/p/2f51144c35c9)里提到的方案，留坑，待学习实践后再分享。  

#### 加速方案
[FinalSpeed](https://github.com/d1sm/finalspeed)是高速双边加速软件,可加速所有基于tcp协议的网络服务,在高丢包和高延迟环境下,仍可达到90%的物理带宽利用率,即使高峰时段也能轻松跑满带宽。详见[IP4A.com](http://www.ip4a.com/c/131.html)，安装以及注意事项请参考：[客户端以及服务端安装教程](https://github.com/d1sm/finalspeed/blob/master/README.md)

# 其他非VPN科学上网方案

#### Shadowsocks
[影梭](https://github.com/shadowsocks)

#### Lantern (P2P)
[蓝灯](https://github.com/getlantern)

#### Tor
[Tor](https://www.torproject.org)

#### gfw.press
[GFW.Press](https://github.com/chinashiyu/gfw.press)

#### 谷歌镜像
[谷歌镜像](https://kuaiguge.com)

# 怀念下曾经的翻墙利器
GoAgent以及所有曾经借助Google App Engine服务翻墙的工具们，毁誉参半吧，实在不好说！  
自由门，涉及政治问题，实在不便说！  

如果以上你实在是不愿意折腾折腾折腾，那么你可以选择买买买！反正这里我是不会荐荐荐的，如果你确实想要要要，那么，你可以去找找找。小道消息，Google Play上有大量的免费VPN，但是前提是，想上Google Play，需要先翻墙。又是一个鸡生蛋蛋生鸡的问题。

# 后记
OpenVPN因为移动客户端(iOS和Android)配置非常麻烦，且翻墙效果不佳，这里不在赘述，感兴趣的朋友可以自行搜索资料学习。  
Openswan据说热度被Strongswan甩了好几条街，所以这里也不介绍了。L2TP和IPSEC文中均采用Strongswan来搭建。  

# 参考资料
感谢网路上各位前辈高人大牛老司机的带路指点，排名不分先后，感恩不分轻重

[为什么有的VPS不能搭建VPN](http://www.worlduc.com/blog2012.aspx?bid=12615260)  
关于PPTP的  
[centos系统下安装VPN(pptp)](http://laibulai.iteye.com/blog/941626)  
[Gentoo搭建PPTP服务器](http://yuan.iteye.com/blog/1170385)  
[CentOS系统下OpenVZ VPS安装PPTP VPN的方法](http://down.chinaz.com/server/201111/1342_1.htm)  
关于L2TP(IPSEC)的  
[IPsec L2TP VPN server](https://wiki.gentoo.org/wiki/IPsec_L2TP_VPN_server)  
[IPSEC L2TP VPN on Ubuntu 14.04 with OpenSwan, xl2tpd and ppp](https://raymii.org/s/tutorials/IPSEC_L2TP_vpn_with_Ubuntu_14.04.html#Install_ppp_openswan_and_xl2tpd)  
关于IPSEC的  
[strongSwan 5 based IPSec VPN, Ubuntu 14.04 LTS and PSK/XAUTH](https://trick77.com/strongswan-5-vpn-ubuntu-14-04-lts-psk-xauth/)  
[如何在 VPS 上搭建 VPN 来翻墙](http://letchinese.com/2015/04/12/build-your-own-vps/)  
关于IKEv2的  
[用 strongSwan 搭建免证书的 IKEv2 VPN](http://blog.zorro.im/posts/strongswan-ikev2-for-ios-without-certificate.html)  
关于证书的  
[Setup IKEv2 On Demand VPN on iOS 8 and IKEv2, IKEv1 Cisco IPSec VPN with Strongswan](https://maoxian.de/2014/10/1220.html)  
[IPSEC VPN on Ubuntu 15.04 with StrongSwan](https://raymii.org/s/tutorials/IPSEC_vpn_with_Ubuntu_15.04.html)  
[strongSwan 5: How to create your own private VPN](https://www.zeitgeist.se/2013/11/22/strongswan-howto-create-your-own-vpn/)  
[iOS8 不越狱翻墙方案](https://songchenwen.com/tech/2014/10/13/cross-fire-wall-on-ios8/)  
[用 Let‘s Encrypt 证书搭建 IKEv2 VPN](http://blog.zorro.im/posts/strongswan-ikev2-for-ios-with-letsencrypt.html)  
[Strongswan on Ubuntu 16.04 for iOS 9 Client](http://dcamero.azurewebsites.net/strongswan-ubuntu-1604-ios-9.html)  
关于Strongswan的  
[Strongswan官方文档](https://wiki.strongswan.org/projects/strongswan/wiki/ConnSection)  
关于客户端配置的  
[iOS (Apple iPhone, iPad...) and Mac OS X](https://wiki.strongswan.org/projects/strongswan/wiki/IOS_(Apple))  
[IKEv2 Configuration Profile for Apple iOS 8 and newer](https://wiki.strongswan.org/projects/strongswan/wiki/AppleIKEv2Profile)  
[iOS Ondemand IPSec VPN Setup](https://medium.com/@cattyhouse/ios-ondemand-ipsec-vpn-setup-ebfb82b6f7a1#.g9spy0itp)  
网络服务加速  
[FinalSpeed](https://github.com/d1sm/finalspeed)  