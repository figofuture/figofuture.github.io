---
layout: post
title:  "零成本打造安全博客的简单办法"
subtitle:   "除了使用Let's Encrypt可以零成本打造安全的博客之外，这里介绍了另一种方法"
date:       2016-04-26
author:     "figotan"
header-img: "img/header/20160427.png"
header-mask: 0.5
catalog: true
tags:
    - letsencrypt
    - http hijacking
    - https
    - ssl
    - gitlab
    - Blog
    - 2016
---

昨天写了一篇介绍[Let's Encrypt实践](http://www.figotan.org/2016/04/26/let-us-encrypt-my-blog/)的文章，但是我的博客是托管在第三方Pages平台的，比如Gitlab，那如何使用https呢？虽然Gitlab的博客里也提到了[用Let's Encrypt来做Pages网站的https化](https://about.gitlab.com/2016/04/11/tutorial-securing-your-gitlab-pages-with-tls-and-letsencrypt/)，但是过程稍微有些繁琐，比如生成证书的过程，而且最不方便的是，没有提供自动化更新证书的办法。  
> When you finish setting up, just put in your calendar to remember to renew the certificate in time, otherwise it will become invalid, and the browser will reject it.
  
这里笔者试验出另一种办法，如果你的博客是托管在Gitlab的Pages上的（Github和Coding.net的Pages目前并没有支持自定义域名的SSL设置），那么可以零（全）成（免）本（费）做一个https化的博客网站。

# 申请沃通免费证书
知乎上有一篇[帖子](https://www.zhihu.com/question/36710815)，无意间介绍了两个提供免费证书的服务商，一个叫[StartSSL](https://www.startssl.com)，另一个是我现在用到的[沃通SSL](https://www.wosign.com/)  
在沃通申请免费证书很简单，步骤如下：

1. 首次注册即可申请，访问[申请免费SSL证书](https://buy.wosign.com/free/)
2. 依次输入证书需要绑定的域名，申请年限，证书语言，登录邮箱，密码，验证码，点击提交即可。  
![](http://www.figotan.org/img/in-post/Snip20160427_1.png)  
3. 证书的生成需要递交CSR文件，目前支持两种，一是提供密码由沃通的后台系统自动生成，二是在自己的服务端生成然后手动上传。因为我自己没有服务端，所以选择第一种方式。  
![](http://www.figotan.org/img/in-post/Snip20160427_2.png)  
4. 递交成功后就可以按照提示下载包含有证书和私钥文件的压缩包了，记住步骤3的密码，解压这个压缩包需要用到。如果证书丢失了，可以再次登录到沃通的web portal取回证书（但是密码一定要记得哦）。

# 证书格式转换
下载压缩包后，双击打开，输入密码，你会发现如下内容

* for Apache.zip
* for IIS.zip
* for Nginx.zip
* for Other Server.zip
* for Tomcat.zip

根据不同的Web Server类型选择不同的子压缩包。那么问题来了，Gitlab的Pages采用的什么样的Web Server呢？用Chrome的开发者工具分析了下自己托管在Gitlab的Pages上的网站，然而并没有发现什么。然后想到Gitlab的Pages自定义域名那里是让用户手动输入证书内容的，所以它用什么Web Server不重要了。  
但是这里，因为对Nginx比较熟悉，就解压看看里面是什么。  
发现有两个文件，一个叫 1_subdomain.your_domain.com_bundle.crt，另一个叫2_subdomain.your_domain.com.key

crt是什么格式？去问了度娘，原来是证书的一种格式，但是Gitlab的Pages只支持pem（证书的另一种格式）格式的证书和私钥，所以一拍脑袋，需要做转换，于是，敲了如下命令做证书格式转换：

```bash
openssl x509 -in 1_subdomain.your_domain.com_bundle.crt -out 1_subdomain.your_domain.com_bundle.pem -outform PEM
```

将**1_subdomain.your_domain.com_bundle.pem**的内容打印出来

```bash
cat 1_subdomain.your_domain.com_bundle.pem
```

然后将终端打印的内容贴到gitlab的pages设置自定义域名的地方（填写好域名，私钥的内容也打印并贴上），点击"**Create New Domain**"，然后Gitlab报错，提示证书格式出错。

这是什么鬼？难道**1_subdomain.your_domain.com_bundle.crt**不是真的crt格式？验证下，直接把**1_subdomain.your_domain.com_bundle.crt**和**2_subdomain.your_domain.com.key**的内容打印并贴到Gitlab，点击"**Create New Domain**"，这次成功了！

由此得到一个教训，直接看后缀名就判断文件格式并不可靠，这里明明是pem格式的证书，后缀却是crt。

那么，如果获得一个文件的真正的格式呢？并没有找到类似的命令，但是发现[一篇文章](http://www.cnblogs.com/guogangj/p/4118605.html)，可以用下面的办法判断

```bash
openssl x509 -in certificate.crt -text -noout
```

如果命令的输出有错误提示，那就不是pem格式的证书了。

# Gitlab上设置证书
登录到Gitlab，选择pages所在的项目，点击"Settings"，然后点击"Pages"，点击"New Domain"，在下图中依次填入域名，证书(pem)和证书私钥(pem)，然后点击"**Create New Domain**"
![](http://www.figotan.org/img/in-post/Snip20160427_3.png)

# 去浏览器看看效果
打开浏览器看看你的网站，是不是多了一把小锁？那么恭喜你，你的博客已经https化了。

# 参考资料
[Hosting on GitLab.com with GitLab Pages](https://about.gitlab.com/2016/04/07/gitlab-pages-setup/)  
[Tutorial: Securing your GitLab Pages with TLS and Let's Encrypt](https://about.gitlab.com/2016/04/11/tutorial-securing-your-gitlab-pages-with-tls-and-letsencrypt/)  
[那些证书相关的玩意儿(SSL,X.509,PEM,DER,CRT,CER,KEY,CSR,P12等)](http://www.cnblogs.com/guogangj/p/4118605.html)  