---
layout: post
title:  "Android插件化框架和热修复技术的资料收集和汇总"
subtitle:   "收集了市面上一些流行的安卓插件化框架和热修复技术的资料，这里主要汇总一下"
date:       2016-08-12
author:     "figotan"
header-img: "img/header/20160812.jpg"
header-mask: 0.5
catalog: true
tags:
    - Android
    - 插件化
    - 热修复
    - 动态部署
    - 2016
---

## 插件化框架  
一个APP功能的堆叠和业务的蓬勃发展，导致APP越来越庞大和臃肿，每一个APP都有一颗超级APP的理想和成为系统第二的愿望，如何减少APP的发布成本和更新成本，插件化的方式是一条不错的捷径。

#### 插件化的介绍与原理
[Android博客周刊专题之＃插件化开发＃](http://www.androidblog.cn/index.php/Index/detail/id/16)  
[Android 插件化 动态升级](http://www.trinea.cn/android/android-plugin/)  
[Android 动态加载技术文章以及相关项目](https://github.com/kaedea/android-dynamical-loading)  
[Android FAQ](https://github.com/wequick/Small/wiki/Android-FAQ)  
[android-dynamic-load-awesome](https://github.com/liaohuqiu/android-dynamic-load-awesome)  
[农民伯伯 Android动态加载jar/dex](http://www.cnblogs.com/over140/archive/2011/11/23/2259367.html)  
[https://github.com/nuptboyzhb/AndroidPluginFramework](https://github.com/nuptboyzhb/AndroidPluginFramework)  
[Android Plugin Framework 插件开发框架及示例程序，原理介绍等](https://github.com/limpoxe/Android-Plugin-Framework)  

#### 开源的插件化框架
[任玉刚 dynamic-load-apk](https://github.com/singwhatiwanna/dynamic-load-apk)  

[bunnyblue OpenAtlas](https://github.com/bunnyblue/OpenAtlas)  
[bunnyblue ACDD](https://github.com/bunnyblue/ACDD)  

[携程 DynamicAPK](https://github.com/CtripMobile/DynamicAPK)  

[Lody Direct-Load-apk](https://github.com/melbcat/Direct-Load-apk)  
[Direct-Load-apk启动插件的原理](http://my.oschina.net/u/2289564/blog/393252)  

[360 DroidPlugin](https://github.com/DroidPluginTeam/DroidPlugin)  
[Android插件化原理解析](http://weishu.me)  

[Small](https://github.com/wequick/Small)  

[android-pluginmgr](https://github.com/houkx/android-pluginmgr)  
[动态加载APK原理分享](http://blog.csdn.net/hkxxx/article/details/42194387)  

[淘宝 Atlas](https://github.com/alibaba/atlas)  

[An open source implementation of MultiAccount](https://github.com/asLody/VirtualApp)

[Qihoo360 RePlugin](https://github.com/Qihoo360/RePlugin)  

[didi VirtualAPK](https://github.com/didi/VirtualAPK)  

## 热修复技术  
云端部署APP补丁程序，客户端检测，下载，合并并应用补丁程序，热修复bug，在用户无需更新下载完整APP的情况下，快速解决用户的问题。  

#### 热修复技术介绍  
[安卓App热补丁动态修复技术介绍](https://mp.weixin.qq.com/s?__biz=MzI1MTA1MzM2Nw==&mid=400118620&idx=1&sn=b4fdd5055731290eef12ad0d17f39d4a&scene=1&srcid=1106Imu9ZgwybID13e7y2nEi#wechat_redirect) QQ空间团队  
[Android dex分包方案](http://my.oschina.net/853294317/blog/308583) 开源中国  

#### 原理分析  
[Android 热补丁动态修复框架小结](http://blog.csdn.net/lmj623565791/article/details/49883661) 鸿洋_  
[Android热更新实现原理](http://blog.csdn.net/lzyzsd/article/details/49843581) 大头鬼Bruce  
[深度理解Android InstantRun原理以及源码分析](https://github.com/nuptboyzhb/AndroidInstantRun)  

#### 开源热修复框架  
[HotFix dodola](https://github.com/dodola/HotFix)  
[RocooFix dodola](https://github.com/dodola/RocooFix)  
[Nuwa 寒江不钓](https://github.com/jasonross/Nuwa)  
[DroidFix BunnyBlue](https://github.com/bunnyblue/DroidFix)  
[AndFix 董炼师](https://github.com/alibaba/AndFix)  
[Tinker 腾讯](https://github.com/Tencent/tinker)  
[ZeusPlugin 掌阅](https://github.com/iReaderAndroid/ZeusPlugin)  
[Robust 美团](https://github.com/Meituan-Dianping/Robust)  
[Aceso 蘑菇街](https://github.com/meili/Aceso)  

#### 热修复方案实践谈  
[各大热补丁方案分析和比较](http://blog.zhaiyifan.cn/2015/11/20/HotPatchCompare/)  

[基于cydia Hook在线热修复补丁方案](http://blog.csdn.net/xwl198937/article/details/49801975)  

[android-dynamic-load-awesome](https://github.com/liaohuqiu/android-dynamic-load-awesome)  

[微信Android热补丁实践演进之路](http://mp.weixin.qq.com/s?spm=a1z2e.7794127.0.0.Q7LExa&__biz=MzAwNDY1ODY2OQ==&mid=2649286306&idx=1&sn=d6b2865e033a99de60b2d4314c6e0a25&scene=0&__nc=1#wechat_redirect)  
