---
layout: post
title:  "小米路由器如何开启Shadowsocks支持"
subtitle:   ""
date:       2016-10-25
author:     "figotan"
header-img: "img/header/20161025.jpg"
header-mask: 0.2
catalog: true
tags:
    - 小米路由器
    - openwrt
    - shadowsocks
    - 翻墙
    - 2016
---

## 写在前面的废话  
我的博客跨过了8月下旬，跳过了整个9月，然后是10月上旬，中旬，这期间似乎发生了很多大事，杭州G20峰会，国庆长假，各地房价暴涨，蓝瘦香菇，却也能阻止我写点东西，也是醉了。再次想爬格子，却发现，MarkDown不怎么会用了，再次蓝瘦，香菇。。。不多矫情了，这不是我的风格，立即马上步入正题。  

## 第一步永远都是刷机  
首先你得有个小米路由器，不要问我为什么，请看标题。我入手的[小米路由器（R2D）](http://www.mi.com/miwifi/)已经好几个月了，功能什么的用的还可以，就是在国内访问谷歌相关的网站以及服务麻烦点，当然，这个问题和路由器并没有多大关系。但是，如果你把翻墙放在路由器，那么，在家上网的时候，就不必挨个设置你的上网终端了。让翻墙就像空气一样透明，手机，pad，电脑畅快呼吸网络的自由，恩，就是为了这个目标。  
小米路由器据专业人士透露是基于[openwrt](https://openwrt.org/)这个路由器开源平台开发的，而openwrt本身支持shadowsocks的扩展，那么，问题就简单了。  
先来刷一个可以远程登录到路由器的固件（只有这样，我们才能够进入到路由器系统内部折腾和定制）。 据说小米路由器开发版固件可以支持安装远程登录工具，官方说稳定版不支持，那么下载安装对应路由器的开发版啰，点击访问[下载地址](http://www1.miwifi.com/miwifi_download.html)  
在浏览器里访问路由器的web portal（配置页面），找到常用设置->系统状态->手动升级，选择刚刚下载到的路由器固件，开始刷机。  

## 刷远程访问工具包
下载[远程访问工具包](http://d.miwifi.com/rom/ssh)，然后按照如下步骤刷入工具到路由器中

* 下载的工具包bin文件复制到U盘（FAT/FAT32格式）的根目录下，保证文件名为miwifi_ssh.bin；
* 断开小米路由器的电源，将U盘插入USB接口；
* 找根细铁丝（图钉，小米手机／爱疯开卡器都可以）按住reset按钮之后重新接入电源，指示灯变为黄色闪烁状态即可松开reset键；
* 等待3-5秒后安装完成之后，小米路由器会自动重启，开启上帝模式

打开命令行终端，试试看效果

`
ssh root@miwifi.com
`

当看到屏幕上输出如下字符图案时，你该小激动一下了，进入上帝模式  
![](http://figotan.org/img/in-post/Snip20161025_1.png)

## 刷Shadowsocks以及配置启动
前面都是准备工作，到这里才开始，热身，准备安装Shadowsocks  
发现刷好的ROM里自带Shadowsocks，试试看能不能用。  

#### 修改配置
登录到路由器  

```
ssh root@miwifi.com
```

进入硬盘中  

```
cd /userdisk/data/
```

在硬盘里建立一个目录用来存放下载的shadowsocks相关的文件  

```  
mkdir /userdisk/data/shadowsocks  
```

进入这个目录  

```
cd /userdisk/data/shadowsocks/
```

下载[配置文件集合](http://cdn.fds.api.xiaomi.com/b2c-bbs/cn/attachment/0622484fd3cf988d3f5c1b7790379d4f.zip)  

```
wget -c http://cdn.fds.api.xiaomi.com/b2c-bbs/cn/attachment/0622484fd3cf988d3f5c1b7790379d4f.zip
```

解压缩  

```
unzip 0622484fd3cf988d3f5c1b7790379d4f.zip
```

修改下Shadowsocks的配置  

```
{
  "server":"服务器地址",
  "server_port":服务器端口号,
  "local_address":"127.0.0.1",
  "local_port":1081,
  "password":"密码",
  "timeout":600,
  "method":"aes-256-cfb",
  "protocol" : "auth_sha1_v2",
  "obfs" : "tls1.2_ticket_auth",
}
```  

将etc下的文件拷贝到/etc下，覆盖原来的文件，为防止意外，在覆盖前记得**备份**之前的文件

#### 启动相关服务  
启动本地域名服务  

```
/etc/init.d/dnsmasq restart
```

启动防火墙  

```
/etc/init.d/firewall restart
```

授予脚本可执行权限  

```
chmod a+x /etc/init.d/shadowsocks
```

启动Shadowsocks脚本

```
/etc/init.d/shadowsocks start
```

开机启动脚本

```
/etc/init.d/shadowsocks enable
```

## 源代码编译Shadowsocks
嫌小米路由器里自带的Shadowsocks版本太旧，想从源代码安装一个定制过的Shadowsocks，那么就这样折腾吧。  

#### 交叉编译环境搭建  
做过嵌入式系统开发的同学应该对这个概念不陌生，没做过的，额，问问谷歌看。这里简单介绍下交叉编译环境的搭建步骤。

首先，你得有一个安装了linux的机器，我的是**Gentoo**  
然后，在工作的目录新建一个子目录叫做"**xiaomi**"，方便把和小米路由器相关的东西都放在一起。

```
cd ~  
mkdir xiaomi  
cd xiaomi  
```

下载并解压缩[小米路由器工具链](http://bigota.miwifi.com/xiaoqiang/sdk/tools/package/sdk_package.zip)  

```
wget -c http://bigota.miwifi.com/xiaoqiang/sdk/tools/package/sdk_package.zip  
unzip http://bigota.miwifi.com/xiaoqiang/sdk/tools/package/sdk_package.zip  
```
编译工具集在**sdk_package/toolchain/bin/**下  

将编译工具集的运行路径加入到系统默认的运行路径中

```
vi ~/.bash_profile
```

在文件结尾添加如下内容并保存

```
XIAOMI_PATH=~/xiaomi/sdk_package/toolchain/bin
export PATH=$XIAOMI_PATH:${PATH}
```

让配置生效

```
source ~/.bash_profile
```

检查工具是否安装好

```
arm-xiaomi-linux-uclibcgnueabi-gcc -v
```

如果看到类似如下输出，那么，恭喜你，环境已经搭建完成

```
Using built-in specs.
COLLECT_GCC=arm-xiaomi-linux-uclibcgnueabi-gcc
COLLECT_LTO_WRAPPER=/home/figo/xiaomi/sdk_package/toolchain/bin/../libexec/gcc/arm-xiaomi-linux-uclibcgnueabi/4.8.2/lto-wrapper
Target: arm-xiaomi-linux-uclibcgnueabi
Configured with: /home/chen/xq/external_toolchain/xiaoqiang_r1d/.build/src/gcc-linaro-4.8-2013.06-1/configure --build=x86_64-build_unknown-linux-gnu --host=x86_64-build_unknown-linux-gnu --target=arm-xiaomi-linux-uclibcgnueabi --prefix=/home/chen/xq/external_toolchain/xiaoqiang_r1d/toolchain/arm-xiaomi-linux-uclibcgnueabi --with-sysroot=/home/chen/xq/external_toolchain/xiaoqiang_r1d/toolchain/arm-xiaomi-linux-uclibcgnueabi/arm-xiaomi-linux-uclibcgnueabi/sysroot --enable-languages=c,c++ --with-arch=armv7-a --with-cpu=cortex-a9 --with-tune=cortex-a9 --with-float=soft --with-pkgversion='crosstool-NG 1.19.0' --enable-__cxa_atexit --disable-libmudflap --disable-libgomp --disable-libssp --disable-libquadmath --disable-libquadmath-support --with-gmp=/home/chen/xq/external_toolchain/xiaoqiang_r1d/.build/arm-xiaomi-linux-uclibcgnueabi/buildtools --with-mpfr=/home/chen/xq/external_toolchain/xiaoqiang_r1d/.build/arm-xiaomi-linux-uclibcgnueabi/buildtools --with-mpc=/home/chen/xq/external_toolchain/xiaoqiang_r1d/.build/arm-xiaomi-linux-uclibcgnueabi/buildtools --with-isl=/home/chen/xq/external_toolchain/xiaoqiang_r1d/.build/arm-xiaomi-linux-uclibcgnueabi/buildtools --with-cloog=/home/chen/xq/external_toolchain/xiaoqiang_r1d/.build/arm-xiaomi-linux-uclibcgnueabi/buildtools --with-libelf=/home/chen/xq/external_toolchain/xiaoqiang_r1d/.build/arm-xiaomi-linux-uclibcgnueabi/buildtools --with-host-libstdcxx='-static-libgcc -Wl,-Bstatic,-lstdc++,-Bdynamic -lm' --enable-threads=posix --enable-target-optspace --disable-multilib --with-local-prefix=/home/chen/xq/external_toolchain/xiaoqiang_r1d/toolchain/arm-xiaomi-linux-uclibcgnueabi/arm-xiaomi-linux-uclibcgnueabi/sysroot --enable-c99 --enable-long-long
Thread model: posix
gcc version 4.8.2 20130603 (prerelease) (crosstool-NG 1.19.0) 
```

#### 源码编译


## 参考资料

[【SS系列教程】小米路由器安装Shadowsocks](http://bbs.xiaomi.cn/t-11547929)  
[小米路由器一键安装Shadowsocks教程（支持全型号）](http://bbs.xiaomi.cn/t-12863925)  
[2016/10/14更新小米路由器2 (R2D) shadowsocks-libev SSR最新版(2.4.8)使用](http://bbs.xiaomi.cn/t-13088506)  
[小米路由器2 (R2D) 交叉编译shadowsocks-libev ssr版](http://www.jianshu.com/p/b1a8443dbe5f)  
[OpenWrt智能、自动、透明翻墙路由器教程](https://softwaredownload.gitbooks.io/openwrt-fanqiang/content/)  
[Dnsmasq的国内域名加速以及域名劫持防护配置](https://github.com/felixonmars/dnsmasq-china-list)  


