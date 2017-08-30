---
layout: post
title:  "实用LINUX定时任务集合"
subtitle:   ""
date:       2017-08-30
author:     "figotan"
header-img: "img/header/20170830.jpg"
header-mask: 0.5
catalog: true
tags:
    - Linux
    - Cron
    - 2017
---

工作中总会遇到一些routine job，为了节约时间成本，我用脚本在机器上某个固定时间段跑任务，提高工作效率（其实并不是因为懒癌，节省的时间可以用来学习，思考，做更多有创造性的事情），收集和整理了一些比较有实用价值的自动化定时脚本以飨读者。

## 脚本的运行流程
脚本的外壳是bash，内部用啥语言编写都OK。定时机制采用Linux中最常见的[Vixie Cron](https://en.wikipedia.org/wiki/Cron)，关于cron如何使用这里不赘述，感兴趣的童鞋可以直接参考cron的[参考资料](https://wiki.gentoo.org/wiki/Cron)。这里说下让定时任务生效的大致流程。

1. 编写bash脚本，具体实例可以参考后面内容描述的。
2. 将写好的脚本拷贝到 /etc/cron.{hourly/daily/weekly/monthly}目录下，具体脚本是按什么周期执行，这个取决于运行脚本的需求。
3. 给脚本添加可执行权限，`$ sudo chmod a+x /etc/cron.{hourly/daily/weekly/monthly}/xxx`
4. 测试脚本是否工作正常，`$ sudo /etc/cron.{hourly/daily/weekly/monthly}/xxx`
5. 更新cron任务，`$ sudo service cron reload`

## 实用脚本收集

#### 利用apt命令让Ubuntu定期自动更新
重度强迫症患者更新狗的福利

```bash
#!/bin/sh
apt-get update
apt-get -y autoclean
apt-get -y upgrade
apt-get -y dist-upgrade
apt-get -y autoremove
apt-get clean
```

#### 定期更新let'sencrypt证书
let'sencrypt提供的https证书只有90天的有效期，人生苦短，请用cron

```bash
#!/bin/sh
/home/figo/certbot/certbot-auto --debug renew --renew-hook "service nginx reload" > /var/log/letsencrypt/renew.log 2>&1
```

* 请注意certbot-auto提供的renew-hook的功能，很方便很强大，👍

#### Gentoo的portge repo定期更新
Gentoo的安装包更新是比较勤快的，毕竟基于源码发布的嘛

```bash
#!/bin/sh
/usr/bin/eix-sync > /var/log/eix-sync.log
```

* 这里只更新安装包的repo，Gentoo安装包的更新不建议自动化，因为源码安装比二进制安装要复杂很多，失败率比较高，中间需要人参与一些安装失败问题的解决（臭名昭著的问题比如unmask, use flag，编译失败，依赖冲突等等）。

TO BE CONTINUE, 敬请期待，更多的脚本实例还在收集和整理中。