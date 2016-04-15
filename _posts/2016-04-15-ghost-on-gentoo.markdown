---
layout: post
title:  "记一次Gentoo下安装ghost的经历"
subtitle:   "使用一个与众不同的Linux到底是一种怎样的体验？是为差异化的技术买单，还是为一直装过的逼埋单？"
date:       2016-04-15
author:     "figotan"
header-img: "img/header/20160415.jpg"
header-mask: 0.5
catalog: true
tags:
    - ghost
    - nodejs
    - gentoo
    - linux
    - blog
    - 2016
---

## 写在前面的话
[ghost](https://ghost.org)是用[nodejs](https://nodejs.org/)语言编写的一个开源博客系统，如同[拍(P)黄(H)片(P)](http://php.net)世界里的[wordpress](https://wordpress.org)。不过，它号称自己有比wordpress更优势的地方，如同这篇它自己的博客里说的那样，[WordPress vs Ghost](https://ghost.org/vs/wordpress/)，我这里简单摘录如下：

* ghost更简单，说人话就是功能更少，但足够用，wordpress功能丰富但未必都用得上
* ghost内置搜索引擎优化和社交分享，而wordpress需要额外安装插件
* ghost安装很简单，只需三步，而wordpress步骤稍微多了些
* ghost更快，因为使用了nodejs，这是人话么？据说扎克伯格说拍(P)黄(H)片(P)是世界上最好的语言，没有之一
* ghost成本更低，其实笔者目前还没感觉到

所以，综上，ghost适合做博客和内容（产品，活动，会议等等）发布展示，而wordpress更适合做内容管理和电子商务，ghost网站上也对比了[Tumblr](https://www.tumblr.com)和[Medium](https://medium.com),为了彰显它的好，那么，还等什么什么呢，赶紧搭一个用呗。
其实对于我们国人来说，最大的好处其实是这个：

* ghost没有被墙，别问我墙是什么，我会告诉你，墙是万里长城，用来保护你的

## 铛铛铛，安装开始
![](http://b54.in/anjr)  
如果不想搭建的童鞋可以自行在[ghost官网](https://ghost.org)上注册，试用14天之后付费就可以了。想在自己的空间，VPS，云搭建的童鞋可以继续往下看。这里前提是假设大家自己租用了网络空间和域名，有关网络空间和域名的选择可以期待我后面的文章来分解。  
整个过程的一切似乎很顺利，直到我碰到[gentoo](https://www.gentoo.org)。  
我最喜欢的两个linux的发行版，一个叫[LFS](http://www.linuxfromscratch.org)，另一个就是gentoo。它们共同的特点是一切从源代码开始，一切从最麻烦的地方开始，开始折腾，直到永远。据说这样可以对linux非常熟悉，打造出来的linux执行效率最高，白手起家玩转linux，但其实并没有，这样玩最费时间，最折腾。貌似很酷，其实很装逼，开发效率低下，浪费人生。  
我的VPS选择了gentoo，而且就懒得换了，更好的选择可能是ubuntu和centos，但是因为懒得换了，所以有了这篇文章，为了我装过的逼，我得一直装下去。  
基本安装过程见[官方文档](http://support.ghost.org/developers/)，非常详细，这里不再赘述，只说说gentoo下的故事。

## 和gentoo的恩恩怨怨
部署到服务器的时候有一步是需要把ghost做成一个后台服务，让它跟着linux系统一起看日出看日落。官方也给出了具体的[实施方案](http://support.ghost.org/deploying-ghost/)，比如用[Forever](https://npmjs.org/package/forever)和[PM2](https://github.com/Unitech/pm2),还可以用[Supervisor](http://supervisord.org/)，但是，这三种方式有额外的步骤和配置，又是一个折腾的过程。最后，官方给出了可以直接写Init Script的方式，一种最原始的配置系统后台服务的办法，那么，问题来了，官方只给了[最流行的linux发行版的初始化脚本参考](https://github.com/TryGhost/Ghost-Config/blob/master/init.d/ghost)，并没有给出gentoo的。  
而gentoo，用的init script内容和ubuntu,centos之类的差异，所以我开始折腾  
关于gentoo的initscripts，[详细的文档](https://wiki.gentoo.org/wiki/Handbook:X86/Working/Initscripts)  
![](https://wiki.gentoo.org/images/f/fa/Gentoo3-616x123.png)  
说说和ubuntu的差异

* 使用的sh为```#!/sbin/runscript```而不是```#! /bin/sh```
* 打印提示用```ebegin 和 eend```取代了```log_daemon_msg 和 log_end_msg ```
* 前方有坑，请留意，如果你的脚本在一开始用了类似这样的代码```PATH=/sbin:/usr/sbin:/bin:/usr/bin``` 那么，就会遇到```ebegin eend```没有定义的错误，原因是默认的PATH被覆盖了，所以，要么改成```PATH=/sbin:/usr/sbin:/bin:/usr/bin:$PATH```，要么去掉这句话
* 启动／停止／重启服务请直接实现这几个函数

``` bash
start() {
}
stop() {
}
restart() {
}
```

而不是写成

``` bash
case "$1" in
    start)
        ;;
    stop)
        ;;
    restart)
        ;;
esac
```
* 对于非标准函数，在定义之前请声明，比如reload函数在定义之前需要这样声明一下```extra_started_commands="reload"```，脚本的标准函数只有这几个

``` bash
depend()
start()
stop()
```

最后，给个我的脚本作为参考  
![](http://b54.in/anjn)  

``` bash
#!/sbin/runscript

DESC="Ghost"
NAME=ghost
GHOST_ROOT=/var/www/ghost
GHOST_GROUP=ghost
GHOST_USER=ghost
DAEMON=/usr/bin/node
DAEMON_ARGS="${GHOST_ROOT}/index.js"
PIDFILE=/var/run/${NAME}.pid
SCRIPTNAME=/etc/init.d/${NAME}
extra_started_commands="reload"
export NODE_ENV=production

start() {
    ebegin "Starting ${DESC} ${NAME}"
    start-stop-daemon --start --quiet \
        --chuid $GHOST_USER:$GHOST_GROUP --chdir ${GHOST_ROOT} --background \
        --pidfile ${PIDFILE} --exec ${DAEMON} --test > /dev/null \
        || return 1
    start-stop-daemon --start --quiet \
        --chuid $GHOST_USER:$GHOST_GROUP --chdir ${GHOST_ROOT} --background \
        --make-pidfile --pidfile ${PIDFILE} --exec ${DAEMON} -- ${DAEMON_ARGS}
    eend $?
}

stop() {
    ebegin "Stopping ${DESC} ${NAME}"
    start-stop-daemon --stop --quiet --retry=TERM/30/KILL/5 \
        --pidfile ${PIDFILE} --exec ${DAEMON}
    rm -f ${PIDFILE}
    eend $?
}

reload() {
    ebegin "Restarting ${DESC} ${NAME}"
    start-stop-daemon --signal HUP --quiet --pidfile ${PIDFILE} \
        --exec ${DAEMON}
    eend $?
}

```

折腾好了，ghost在gentoo下正常跑起来了，装逼完毕，收宫。
  
## 总结
![](http://b54.in/anjp)  
装逼需小心，坑不会少；  
gentoo的init script和ubuntu，centos的区别是有的，大小自己看，反正不修改是用不起来的；  
既然选择了gentoo,就一直用下去，至少gentoo逼LFS好的地方是，它至少有一套完整的系统软件在为这个系统服务，比如emerge包管理软件和portage软件仓库；  
既然选择了装逼，就不要怕折腾  
> Play or Die  
> Compiling forever more than 10 years
> 

## 鸣谢
[LFS](http://www.linuxfromscratch.org)  
[Gentoo](https://www.gentoo.org)  
[Node.js](https://nodejs.org)  
[Ghost](https://ghost.org)  