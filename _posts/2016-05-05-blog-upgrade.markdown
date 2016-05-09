---
layout: post
title:  ""
subtitle:   ""
date:       2016-05-04
author:     "figotan"
header-img: "img/header/20160504.jpg"
header-mask: 0.5
catalog: true
published: false
tags:
    - github
    - gitlab
    - webhook
    - openshift
    - https
    - dnspod
    - 2016
---

# 问题的产生
博客放在Coding.net和Github一段时间了，期间遇到几个问题，整理如下：

* coding.net有时候不稳定，访问速度甚至不如github，评测如下图
![](http://www.figotan.org/img/in-post/Snip20160505_8.png)  
* 因为webhook的代码放在Openshift（已在墙外），所以利用webhook做代码同步的失败率较高，如下图记录，20次push/test，5次失败，失败率高达25%
![](http://www.figotan.org/img/in-post/Snip20160505_10.png)  
* 博客没有绿色小锁(https)的保驾护航，在写https相关文章的时候会被读者喷，诟病和质疑

# 切换DNSPod分流
针对第一个问题，决定在dns里把博客的域名地址CNAME到Github，但因为Github是搜索引擎爬虫免疫的，所以Coding.net在DNSPod里保留了下来，分流到的**线路类型**为**搜索引擎**。  

# 更换代码同步的来源和目标
对于问题二来说，如果网站放Github，webhook到同样是国外的Openshift感觉问题会少很多，所以决定把Github作为代码托管的主站，然后webhook里改成由Github同步到Coding.net，Coding.net由主站转为代码备份的仓库。[来龙去脉请先阅读笔者的前一篇文章](http://www.figotan.org/2016/03/29/how-to-speed-up-your-blog-using-duplex-git-pages/)。  

1. 修改／编写webhook同步脚本

```
<?php
if( $_GET['key'] == 'KEY' ) {
    echo shell_exec('cd $OPENSHIFT_DATA_DIR/figotan;ssh-agent bash -c "ssh-add /var/lib/openshift/56fb794b2d52711afb000050/app-root/data/.ssh/id_rsa; git pull gh-origin master:coding-pages";ssh-agent bash -c "ssh-add /var/lib/openshift/56fb794b2d52711afb000050/app-root/data/.ssh/id_rsa; git push origin coding-pages"');
}
else {
    header('HTTP/1.1 400 Bad Request');
    echo <<<HTML
// Fallback
HTML;
}
```
注意**KEY**设置成一串随机的字符串，为了一定的安全性考虑。记得上面步骤中php api源码里的**KEY**和webhook中的**KEY**内容一定要一致。

2. 将代码提交到Openshift的项目中
3. 将Coding.net中webhook删除
4. 设置Github博客仓库的webhook
5. 修改下博客代码，并将改动提交到Github，查看Coding.net仓库中代码是否同步更新

# 全站https化
问题三，Github也解决不了了，而且博客页面现在少了备胎，总感觉不踏实。  

所以，博客https化和转向托管到Gitlab正式提上了日程。
