---
layout: post
title:  "安全类网站集锦"
subtitle:   "个人收集的安全类网站列表，以飨读者。"
date:       2016-07-19
author:     "figotan"
header-img: "img/header/20160719.jpg"
header-mask: 0.5
catalog: true
tags:
    - Security
    - Blog
    - 2016
---

因为工作的关系，之前收集过的一些比较不错的关于安全方面知识和实践的网站资源，统一放在这篇文章里，方便大家查阅。还没有想好如何更好的分门别类，所以，暂时凌乱的放着吧。  

## Android  
移动端，特别是安卓接触的时间和机会较多，所以我优先说说安卓下的安全网站资源和项目。  

#### APK爬虫  
如果是分析安卓应用的安全问题，那么第一步应该是：获取大量的app来分析啰！我们一般会编写爬虫来获取大量的app信息以及文件。  
[WebAPKCrawler](https://github.com/Fuzion24/WebAPKCrawler)  
[playdrone-kitchen](https://github.com/nviennot/playdrone-kitchen)  
[google play crawler](https://github.com/Akdeniz/google-play-crawler)  
[android apps crawler](https://github.com/mssun/android-apps-crawler)  
[google play api](https://github.com/egirault/googleplay-api)  

#### 应用行为分析  
拿到app后做什么呢？行为分析呀！  
[Intent Analysis](https://github.com/smee/IntentAnalysis)  
[Soot infoflow](https://github.com/lilicoding/soot-infoflow-android-iccta)  
[NDroid 动态数据流分析](https://github.com/0-14N/NDroid)  
[uiautomator](https://github.com/xiaocong/uiautomator)  
[mobile sandbox tools](https://github.com/mspreitz/mobile-sandbox)  
[AndroidViewClient](https://github.com/dtmilano/AndroidViewClient)  
[Introspy-Android](https://github.com/iSECPartners/Introspy-Android)  
[APKSmash 在APK中寻找敏感信息](https://github.com/intrepidusgroup/APKSmash)  
[androwarn 简单数据流分析](https://github.com/maaaaz/androwarn)

#### 逆向调试  
必要的时候，对于一些复杂的行为，很难从黑盒分析方式得到实质，所以需要反编译看代码或者动态调试分析行为。  
[android lkms](https://github.com/strazzere/android-lkms)  
[ZjDroid](https://github.com/BaiduSecurityLabs/ZjDroid)  
[adbkit](https://github.com/CyberAgent/adbkit)  
[NinjaDroid](https://github.com/rovellipaolo/NinjaDroid)  
[android-scripts](https://github.com/strazzere/android-scripts)  
[jadx dex to java](https://github.com/skylot/jadx)  
[Luyten](https://github.com/deathmarine/Luyten)  
[google版的dex2jar](https://github.com/google/enjarify)  

#### 钩子  
给目标系统或者目标应用挂钩子，是一种相对高阶并且非侵入的分析办法，相对于暴力破解／分析来说，当然，钩子是双向的，进可攻，退可守。  
[ZHookLib](https://github.com/cmzy/ZHookLib)  
[Android-Rootkit](https://github.com/hiteshd/Android-Rootkit)  
[hooker](https://github.com/AndroidHooker/hooker)  
[adbi](https://github.com/crmulliner/adbi)  
[ddi](https://github.com/crmulliner/ddi)  
[ldpreloadhook](https://github.com/poliva/ldpreloadhook)  
[Xposed](https://github.com/rovo89/Xposed)  
[XposedInstaller](https://github.com/rovo89/XposedInstaller)  
[redexer Dalvik instrumentation](https://github.com/plum-umd/redexer)  
[inDroid](https://github.com/romangol/InDroid)  
[IGLogger](https://github.com/intrepidusgroup/IGLogger)  
[Disabler](https://github.com/miktam/Disabler) 

#### 广告检测  
广告行为分析。  
[ADDetector](https://github.com/BaiduSecurityLabs/AdDetector)  

#### 应用加固  
安全的大方向，除了攻，便是防。加固说的通俗点就是给系统或者应用穿盔甲，执盾牌。  
[AndroidObfuscation-NDK](https://github.com/Fuzion24/AndroidObfuscation-NDK)  
[obfuscator](https://github.com/obfuscator-llvm/obfuscator)  
[dehorser](https://github.com/strazzere/dehoser)  

#### 模拟器检测与对抗  
攻击者很多时候为了大规模自动化攻击，可能会用模拟器。比如变换ip地址，变换imsi／imei号码刷接口等恶意操作，所以，模拟器的监测和防护，是一种比较重要的保护手段。毕竟，普通的用户一般不会拿模拟器来访问app的内容。  
[AndroidEmulatorDetection](https://github.com/Fuzion24/AndroidEmulatorDetection)  
[anti-emulator](https://github.com/strazzere/anti-emulator)  

#### 重打包检测  
攻击者破解目标应用插入恶意代码后重新打包并发布到渠道，诱导用户下载安装使之中招，这是一种比较低劣且常见的攻击方式，需要有效的监测并防止。（重打包不一定是为了散布恶意软件或者病毒木马，也可能是为了动态调试目标应用的目的，比如竞品分析之类，所以也是一种反恶意调试的盾）  
[FSquaDRA](https://github.com/zyrikby/FSquaDRA)  

#### 漏洞POC及修复  
利用系统以及应用漏洞，可以用来制作病毒木马，散布恶意程序，窃取用户隐私。也可以用来保护系统以及应用。更可以用来完成一些不可能完成的任务。刀在这里，杀人，救人，还是变魔术，看你自己了。  
[FakeId](https://github.com/Tungstwenty/FakeIDFix)  
[AndroidZipArbitrage](https://github.com/Fuzion24/AndroidZipArbitrage)  

#### 伪基站检测  
伪基站好比一家黑店，你进去买东西（上网购物），焉有有不挨宰（黑）的道理？ 最可恨的是，这家黑店是无形的，看不见摸不着，账户上的钱不见了，可能还没有意识到。  
[Android IMSI Catcher Detector](https://github.com/SecUpwN/Android-IMSI-Catcher-Detector)    

#### 其他  
[安卓每个版本的android.jar收集](https://github.com/Sable/android-platforms)  
[模拟手机获取android_id](https://github.com/nviennot/android-checkin)  
[安卓应用分发列表，包括官方市场渠道以及来自中国大陆，台湾和俄罗斯的第三方分发市场以及渠道](https://github.com/mssun/android-markets-list)  
[Cloud Xiao整理的安全资源列表](https://github.com/secmobi/wiki.secmobi.com)  
[android-security-awesome](https://github.com/ashishb/android-security-awesome)  
[2016 黑客的 Android 工具箱都有哪些？](http://www.oschina.net/news/70908/2016-android-hacker-toolkit)  
[Hacked Team](https://github.com/hackedteam)  
[A database of published security advisories reported by the Programa STIC Team at Fundación Sadosky](https://github.com/programa-stic/security-advisories)  

## OSX & iOS  
[OSX和iOS系统相关安全工具](https://github.com/ashishb/osx-and-ios-security-awesome)  

## Web以及服务端安全  

#### 国内  
[t00ls](https://www.http://t00ls.net)  
[<?php $head = "80vul";](http://www.80vul.com/)  
[Worm.cc](http://worm.cc/)  
[FreeBuf.COM 关注黑客与极客](http://www.freebuf.com/)  
[Sebug漏洞库: 漏洞目录、安全文档、漏洞趋势](http://sebug.net/)  
[网络安全攻防研究室](http://www.91ri.org/)  
[SecWiki-安全维基,汇集国内外优秀安全资讯、工具和网站](http://www.sec-wiki.com/)  
[腾讯安全应急响应中心](http://security.tencent.com/index.php/blog)  
[黑吧安全网 - 中国最早的IT技术门户网 成就IT精英 网络安全](http://www.myhack58.com/)  
[看雪安全论坛](http://bbs.pediy.com/)  
[WooYun知识库](http://drops.wooyun.org/)  
[WooYun.org | 自由平等开放的漏洞报告平台](http://www.wooyun.org/)  
[吾爱破解论坛-LCG-LSG|软件安全](http://www.52pojie.cn/)  

#### 国外  
[exp漏洞库](http://www.exploit-db.com/)  
[路由器漏洞专区](http://routerpwn.com/)  
[Paper汇总](http://www.secdocs.org/)  
[..:: Corelan Team](https://www.corelan.be/)  
[ha.ckers.org web application security lab](http://ha.ckers.org/)  
[SpiderLabs Anterior](http://blog.spiderlabs.com/)  
[CVE - Common Vulnerabilities and Exposures (CVE)](http://www.cve.mitre.org/)  
[http://http://news.hitb.org/](http://news.hitb.org/)  
[Home | Sucuri Blog](http://blog.sucuri.net/)  
[Help Net Security](http://www.net-security.org/)  
[Blogs | The Honeynet Project](http://www.honeynet.org/)  
[We Are Legion](http://wearelegionthedocumentary.com/)  
[The Hacker News](http://thehackernews.com/)  
[Threatpost | The first stop for security news](http://threatpost.com/)  
[www.ex-parrot.com](http://www.ex-parrot.com/)  
[Ctrlbox.com](http://www.cyberwarnews.info/)  
[AnonNews.org : Everything Anonymous](http://anonnews.org/)  
[http://sla.ckers.org/forum/](http://sla.ckers.org/forum/)  
[Hack This Site Forum • Index page](http://www.hackthissite.org/forums/)  
[Awesome Security](https://github.com/sbilly/awesome-security)  
[application security](https://github.com/paragonie/awesome-appsec)  
[Awesome CTF](https://github.com/apsdehal/awesome-ctf)  
[Awesome Malware Analysis](https://github.com/rshipp/awesome-malware-analysis)  
[Awesome Hacking](https://github.com/carpedm20/awesome-hacking)  
[Awesome Honeypots](https://github.com/paralax/awesome-honeypots)  
[awesome-incident-response](https://github.com/meirwah/awesome-incident-response)        

## 参考链接
[安全类网站推荐列表](http://daily.zhihu.com/story/3877456)  
[Awesome Security](https://github.com/sbilly/awesome-security)  