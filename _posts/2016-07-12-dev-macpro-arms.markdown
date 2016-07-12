---
layout: post
title:  "开发者MAC电脑里的十八般兵器"
subtitle:   "作为一名开发者，无论是移动端(Android/iOS)，前端(H5)，后端(PHP/JAVA/GO)或者全栈，运用常用工具辅助开发，提高编程，调试，以及抓八阿哥的效率。如同古人习武，十八般兵器，样样精通，方能小有成就。"
date:       2016-07-12
author:     "figotan"
header-img: "img/header/20160712.jpg"
header-mask: 0.5
catalog: true
tags:
    - MAC
    - Android
    - tools
    - blog
    - 2016
---

古人常以刀、枪、剑、戟、斧、钺、铲、叉、鞭、锏、锤、戈、镋、棍、槊、棒、矛、钯十八种兵器，样样精通，来形容一个人的武学技能get状态。在开发者的世界里，熟练掌握各种辅助工具，可以达到事半功倍，快速提高工作效率的效果。闲话不扯了，来看看究竟是什么。

## 道场
习武学道讲经论法总有个场所，这样容易把有意向来学习的人聚集起来，而且有助于信息的传播，精力的集中，技能的修炼，经验的交流和水平高下的切磋。工具的运用也是在一个特定的开发环境里才能发挥出比较好的效果。  
我这里的开发环境(DOJO)是苹果公司Apple Inc.()2014年出的一款Mac Pro，具体配置如下：  

```
MacBook Pro（Retina 显示屏，15 英寸，2014 年中）
处理器 2.2 GHz Intel Core i7
内存 16 GB 1600 MHz DDR3
图形卡 Intel Iris Pro 1536 MB
操作系统 OS X EI Capitan
```

这里描述的工具都是运行在这个硬件以及软件环境的，经过了2年多的开发经验／经历的靠谱验证，所以拿来分享给大家。  
**如果有朋友的研发环境和这里描述的不太一致，那么仅作参考吧，具体结合自己的情况。**

为了方便检索，所以增加了工具分类。

开发第一步是做什么？学习文档？画流程图？还是直接写代码？恩，先从学习文档开始吧。

## 文档查看

#### Dash
写代码的时候是不是有些API记不住，比如画椭圆该用哪个类？计算开平方用什么函数？怎么连接远程的mysql服务器检索数据？这个时候一般怎么办？问度娘？问谷歌？直接查看在线编程文档？  
在国内问谷歌需要翻墙，那么涉及到另外工具的使用。查看在线文档，如果记不住入口网址怎么办？放收藏里啊，如果入口改变了呢？还是需要问搜索引擎啊！那么问题来了，度娘乱贴小广告咋办？用Dash吧，一个APP搜罗了这个世界上几乎所有的编程语言文档，而且更新速度快。  
软件主页以及下载地址[https://kapeli.com/dash](https://kapeli.com/dash)  

## 流程图设计
OmniGraffle有很多人推荐，不过笔者觉得这个软件太贵了，所以推荐了两款免费的软件流程设计工具。

#### XMind
主攻脑图（思维导图），流程图也支持，另外还有日程安排计划等额外的功能。  
软件主页以及下载地址[http://www.xmind.net/](http://www.xmind.net/)

#### Gliffy Diagrams
并不是一个独立安装的APP，而是作为Chrome的插件，可以去Chrome的App Store下载安装，很轻量，运行速度快。  
软件主页[https://www.gliffy.com/](https://www.gliffy.com/)

## 文本编辑器
不仅限于代码编辑，一款好的编辑器会让你的编辑工作充满愉悦。

#### MacVim
为什么我一开始不推荐时下流行头牌Sublime呢？因为，我用vi/vim已经超过十年的时间了。当初在学校，vm/emacs二选一，我选择了更容易上手实践的vi，从此一直用它来查看／编译文本／代码。  
软件主页以及下载地址[http://macvim-dev.github.io/macvim/](http://macvim-dev.github.io/macvim/)

#### MacDown
一般代码查看和编辑用Vi就够了，剩下其他的文档，恩，现在大多数文章／文档采用的**MarkDown**语法编写，所以用一款MarkDown编辑器就够了。比如本文的编写，我用的MacDown编辑器，文章语法采用MarkDown语法。既然是MarkDown编辑器，那么有人会提到用**Mou**，笔者也试用过一段时间，遇到了一些问题，比如语法支持和界面显示，后来改用MacDown，觉得各方面都支持的不错，所以一直使用。  
软件主页以及下载地址[http://macdown.uranusjr.com/](http://macdown.uranusjr.com/)

#### Sublime Text
如果你不是一路走着linux从事开发的话，估计很难对Vi/Emacs熟悉。那么，像note++或者ultraedit这类第三方编辑器会是你比较不错的选择。相比于集成开发环境IDE的笨重，运行慢和耗内存，选择一个轻量级的编辑器是在平时比较频繁的非常规查看／编辑代码／文档时一个不错的选择。那么，以前那些用第三方编辑器的用户都去哪儿了？应该就是这个Sublime Text了吧。  
软件主页以及下载地址[http://www.sublimetext.com/](http://www.sublimetext.com/)

## 图片编辑器
写文章撸代码，除了文字的处理外，还需要有美图的点缀和衬托。更多时候，图是吸引流量和眼球的一种重要手段。

#### GIMP
为啥不用Adobe Photo Shop呢？太贵，太复杂。那么，好吧，这里笔者推荐用GIMP，PS该有的，它基本都有。  
软件主页以及下载地址[http://www.gimp.org/](http://www.gimp.org/)

## 集成开发环境IDE
集成开发环境一般是集编辑，编译，链接，调试，版本管理和打包发布于一体的大型开发软件。它的特点是功能丰富，上手快，易操作。缺点也显而易见，笨重，运行速度慢，需要更多的CPU，内存资源。

#### Eclipse
老牌万金油型集成开发环境，上手快，支持几乎所有语言，但是近几年使用人数在下滑，逐渐转向Android Studio和IntelliJ IDEA了。  
软件主页以及下载地址[http://www.eclipse.org](http://www.eclipse.org)

#### Android Studio
安卓程序猿专属开发环境。  
软件主页以及下载地址[https://developer.android.com/studio/index.html](https://developer.android.com/studio/index.html)

#### IntelliJ IDEA
Eclipse替代品，支持市面上大部分流行的开发语言和框架，上手快，界面更加人性化，现代集成开发环境的典范。  
软件主页以及下载地址[https://www.jetbrains.com](https://www.jetbrains.com)

#### Xcode
苹果公司官方唯一指定的Object-C与Swift集成开发环境。  
软件主页以及下载地址[https://developer.apple.com/xcode/](https://developer.apple.com/xcode/)

## 分析调试类
APP写好了，安装到设备，但是从网络拉取图片显示失败了，怎么破？APP打安装包后想看下包里面到底有些啥？遇到这样的问题，这个的工具可以帮助你解决上面遇到的问题。

#### Wireshark
老牌网络抓包利器，各种平台都可以玩耍。  
软件主页以及下载地址[https://www.wireshark.org/](https://www.wireshark.org/)

#### tcpdump
这是一个命令行工具，可以看作是Wireshark的命令行版。  
系统自带，无需额外安装。[使用帮助](https://support.apple.com/en-hk/HT202013)

#### Charles
网络抓包利器加上代理功能，并支持自签名证书，所以可以用来在手机上抓取https的包。使用非常方便。付费软件，值得购买。  
软件主页以及下载地址[https://www.charlesproxy.com/](https://www.charlesproxy.com/)

#### JD-GUI
Java的class文件反编译神器，可以从二进制class文件查看它的Java源代码。  
软件主页以及下载地址[http://jd.benow.ca/](http://jd.benow.ca/)

#### JADX
JD-GUI的增强版，支持查看安卓apk/dex文件中反编译的Java源代码以及查看apk中其他文件的内容。  
软件主页以及下载地址[https://github.com/skylot/jadx](https://github.com/skylot/jadx)

## 版本管理
频繁的修改，反悔，记录需要管理，所以版本管理是必须的。

#### SourceTree
Atlassians出品的图形化版本管理工具，支持Git和Mercurial。  
软件主页以及下载地址[https://www.sourcetreeapp.com/](https://www.sourcetreeapp.com/)

## 文件共享
从文件服务器(FTP, Samba etc.)下载资料或者上传文件到服务器上。

#### FileZilla
老牌Sourceforge开源文件传输软件。  
软件主页以及下载地址[https://sourceforge.net/projects/filezilla/](https://sourceforge.net/projects/filezilla/)

## 证书管理
证书一般用于https加密，移动APP软件的安装文件签名。

#### Portecle
图像化管理证书的工具。  
软件主页以及下载地址[https://sourceforge.net/projects/portecle/](https://sourceforge.net/projects/portecle/)

## 截屏
截屏是强需求，没错。MAC下有截屏快捷键，只能截屏。一般用户截屏完毕后，不是马上发出去，而是做后期处理。

#### snip
截屏，编辑。  
软件主页以及下载地址[http://snip.qq.com/](http://snip.qq.com/)

## 数据库
调试APP的时候，如果APP产生了数据，并且把数据保存在数据库（sqlite）中。如果想在开发主机上查看，可以用如下的工具。

#### Datum
查看sqlite数据库的内容。  
软件主页以及下载地址[http://www.datumapps.com/](http://www.datumapps.com/)

## 网络请求
有时候需要自己构造一个http网络请求(GET/POST)，并查看输入输出的详细内容。简单的GET用浏览器可以代劳，复杂一点的需要浏览器安装插件支持。用如下的工具可以到达更好的效果。

#### wget
命令行工具。除了查看发送网络请求，查看结果外。另外一个用途是下载文件，特别是大文件，用浏览器下载经常会断线，而且断点续传做的不是太好。wget命令下载文件，支持断点续传，这个用起来不错。

#### curl
功能基本同wget，系统自带工具，无需安装。

#### rest-client
支持restful风格的网络请求构造，请求和结果相应。调试restful接口的好帮手。  
软件主页以及下载地址[https://github.com/wiztools/rest-client](https://github.com/wiztools/rest-client)

## 虚拟机&模拟器
我的电脑是MAC，可是招商银行的专业版没有MAC的客户端，肿么破？我想在MAC上看到安卓APP运行的情况，怎么办？安装一个虚拟机吧！

#### VirtualBox
老牌虚拟机软件，支持市面上几乎所有流行的操作系统。  
软件主页以及下载地址[https://www.virtualbox.org/](https://www.virtualbox.org/)

#### Genymotion
安卓模拟器，运行安卓APP如同在手机上一样的速度。  
软件主页以及下载地址[https://www.genymotion.com/](https://www.genymotion.com/)

## MAC专用
有些工具是MAC系统专用的，比如用来管理苹果设备(iPad, iPhone, iMac, Mac etc.)配置文件的工具。

#### Apple Configurator
上App Store自行搜索下载安装。  
[使用帮助](http://help.apple.com/configurator/mac/2.2.1/)

## 服务端工具套件
有时候需要本地调试一些服务端提供的服务，或者是网站后台。这个时候一个开发／调试／模拟环境的选择变的重要了。还是那句不忘初衷的话，好的工具让你事半功倍！

#### Bitnami服务端套件
本地调试web服务器，nginx, mysql, php-fpm, etc.  
软件主页以及下载地址[https://bitnami.com](https://bitnami.com)

#### Kitematic
Docker图形化管理工具。
软件主页以及下载地址[https://kitematic.com/](https://kitematic.com/)

## 翻墙利器
我要上谷歌搜索最新的Android开发文档和API，可是目前在国内用不了谷歌，怎么办？翻墙吧！

#### ShadowsocksX
看标题，不多说，默默的下载，安装然后运行，配置，打开浏览器，访问谷歌，搜索Android就可以啦！  
软件主页以及下载地址[https://sourceforge.net/projects/shadowsocksgui/](https://sourceforge.net/projects/shadowsocksgui/)

#### Lantern
如果上面那个不行，那么试试这个吧，不多说了。  
软件主页以及下载地址[https://github.com/getlantern/lantern](https://github.com/getlantern/lantern)








