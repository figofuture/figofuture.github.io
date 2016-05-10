---
layout: post
title:  "博客优化记"
subtitle:   "升级装备，加红加蓝，上护甲"
date:       2016-05-10
author:     "figotan"
header-img: "img/header/20160510.png"
header-mask: 0.5
catalog: true
published: true
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
    echo shell_exec('cd $OPENSHIFT_DATA_DIR/figotan;ssh-agent bash -c "ssh-add $OPENSHIFT_DATA_DIR/.ssh/id_rsa; git pull gh-origin master:coding-pages";ssh-agent bash -c "ssh-add $OPENSHIFT_DATA_DIR/.ssh/id_rsa; git push origin coding-pages"');
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

#### Github代码同步到Gitlab上
首先在Gitlab上新建一个工程，名字叫${USERNAME}.gitlab.io, **${USERNAME}**换成你自己在Gitlab上的用户名。这个工程是作为用户页面的工程，所以页面源代码可以放在master分支下，这样的话，分支和Github相同了（后面就知道好处了）。  

进入Github页面代码目录，修改文件**.git/config**

```
[remote "origin"]
        url = git@github.com:${USERNAME}/${USERNAME}.github.io.git
        url = git@gitlab.com:${USERNAME}/${USERNAME}.gitlab.io.git
        fetch = +refs/heads/*:refs/remotes/origin/*
```

修改后保存，正常修改，提交博客代码，一次命令可以产生两次提交，分别同步到Github和Gitlab，是不是很方便？

#### Gitlab页面配置
在浏览器里访问${USERNAME}.gitlab.io.git，发现报404的错误。Gitlab的页面需要有一个页面配置文件

在博客源码根目录下添加**.gitlab-ci.yml**，如果是用的**Jekyll**，内容参考如下

```
image: ruby:2.1             # the script will run in Ruby 2.1 using the Docker image ruby:2.1

pages:                      # the build job must be named pages
  script:
  - gem install jekyll      # we install jekyll
  - gem install jekyll-paginate
  - jekyll build -d public/ # we tell jekyll to build the site for us
  artifacts:
    paths:
    - public                # this is where the site will live and the Runner uploads it in GitLab
  only:
  - master                  # this script is only affecting the master branch
```

详细文件语法以及其他静态博客（Hexo）的配置请参考[Gitlab Pages的官方文档](http://doc.gitlab.com/ee/pages/README.html)。

#### 自定义域名
Gitlab Pages里添加自定义域名，并在DNS供应商那里将域名**CNAME**到**${USERNAME}.gitlab.io.git**

#### https证书申请
按照Gitlab的博客文章[Tutorial: Securing your GitLab Pages with TLS and Let's Encrypt](https://about.gitlab.com/2016/04/11/tutorial-securing-your-gitlab-pages-with-tls-and-letsencrypt/) 用Let's Encrypt生成证书没有成功，所以放弃改用[沃通](https://www.wosign.com)免费证书，目前提供了为期两年的免费SSL证书，[申请地址](https://buy.wosign.com/free/)，具体过程参考我的另一篇文章：[零成本打造安全博客的简单办法](http://www.figotan.org/2016/04/26/using-free-wosign-to-certificate-your-blog-on-gitlab/)

注意：分别给**www.${YOUR_DOMAIN_NAME}**和**${YOUR_DOMAIN_NAME}**申请证书并配置到Gitlab的Pages中。

用浏览器访问你的网站，用https://前缀，是不是看到地址栏上有一把绿色小锁（某些浏览器不会显示绿色）了？

#### 页面内容
在**_config.yml**文件的**url**字段将http改成https，因为url在head.html中被标记成**canonical**，所以搜索引擎的所有流量会流到https  
对于网页内资源的引用，比如css,js,jpeg/png等等，将http改成https，也可以改成协议无关的url格式

```
<link rel="stylesheet" href="//${YOUR_DOMAIN_NAME}/styles.css" />
<script src="//${YOUR_DOMAIN_NAME}/script.js"></script>
```

对于js重定向来说，可以采用下面这种方式

```
var host = "${YOUR_DOMAIN_NAME}";
if ((host == window.location.host) && (window.location.protocol != 'https:')) {
  window.location = window.location.toString().replace(/^http:/, "https:");
}
```

最后，可以用[SSL Labs提供的免费在线工具分析分析博客的SSL配置情况](https://www.ssllabs.com/ssltest/)呢！

# 参考资料

[Tutorial: Securing your GitLab Pages with TLS and Let's Encrypt
](https://about.gitlab.com/2016/04/11/tutorial-securing-your-gitlab-pages-with-tls-and-letsencrypt/)  
[The Protocol-relative URL](http://www.paulirish.com/2010/the-protocol-relative-url/)  
[SSL Server Test](https://www.ssllabs.com/ssltest/)
