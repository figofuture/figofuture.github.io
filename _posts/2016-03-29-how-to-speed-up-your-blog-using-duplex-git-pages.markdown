---
layout: post
title:  "一切从搭建博客开始谈起"
subtitle:   "写在2016年的第一篇，如何利用国内外双git pages平台让你的博客访问飞一般的快"
date:       2016-03-29
author:     "figotan"
header-img: "img/header/Wallpaper-2015-12-22.jpg"
header-mask: 0.5
catalog: true
tags:
    - github
    - coding.net
    - gitcafe
    - dnspod
    - blog
    - 2016
---
## 问题描述
静态博客托管在github上诚然是一件非常不错的事情，但是github在国内访问速度很慢，而且经常因为违反国内的政策被墙而导致无法访问，所以在国内找一个类似github的代码托管兼静态页面托管平台并采用双托管方式似乎是个不错的解决方案。

## 解决方案
国内有个叫gitcafe的代码托管平台提供了类似github的服务，不过gitcafe已经整合到coding.net，所以国内选择了coding.net作为代码和静态网页的托管。
国内的静态网页托管问题解决了，但是coding.net有自己的默认域名和绑定域名的方式，如何与github绑定的域名共存呢？也就是对于国内的用户让他们访问coding.net托管的网页；对于海外的用户，让他们访问github托管的网页，而且用户在浏览器上敲下的网址要都是同一个。其实[采用CDN的方式](http://www.dozer.cc/2015/06/github-pages-and-cdn.html)可以解决这个问题，但是免费靠谱的CDN资源非常难找，而且也有诸多限制，这里采用了免费的DNSPod服务，可以将国内的dns请求解析到国内的服务器(coding.net)，国外的dns请求解析到国外的服务器(github.com)，有效的解决了问题。
下面拿本人的博客[http://www.figotan.org](http://www.figotan.org)举例说明如何一步一步解决这个问题。

## 国内代码以及静态网页托管平台－coding.net
访问 [http://www.coding.net](http://www.coding.net)注册账号，记得账号名最好和github的账号名保持一致（不一致其实也没问题）；  
1. 新建并上传一个SSH公钥，操作过程和github类似；  
2. 创建Coding Pages，具体流程可以参考[官方文档](https://coding.net/help/doc/pages/index.html)；  
3. 绑定自定义的网址，如果是主域名访问，比如figotan.org，记得www和非www都需要绑定一下，否则非www的方式，比如http://figotan.org会报404错误；  
4. 在浏览器里访问[http://figotan.coding.me](http://figotan.coding.me)试试看效果

## DNSPod
采用[DNSPod](https://www.dnspod.cn)提供的智能域名解析服务，可以按国内网络供应商（电信，联通，移动，教育网），地域（国内，国外），搜索引擎（谷歌，百度，搜搜，有道，必应，搜狗，奇虎）等维度对线路类型做域名解析。这样，一个域名可以针对不同的用户群体提供不同的空间与之提供服务。国内的用户访问国内的空间，国外的用户访问国外的空间。  
下面是具体的操作步骤和注意的问题：  
1. 注册账号并添加域名；  
2. 添加coding.net的空间地址，具体配置如下图：  ![](http://www.figotan.org/img/in-post/Snip20160328_5.png)  
3. 去域名供应商处改写NS纪录，我用的[狗爹](https://www.godaddy.com/)提供的域名服务，具体修改操作如下图：  ![](http://www.figotan.org/img/in-post/Snip20160328_3.png)  ![](http://www.figotan.org/img/in-post/Snip20160328_4.png)  
4. 刷新本机dns缓存并在浏览器测试效果  
`$ sudo dscacheutil -flushcache`  
`$ sudo killall -HUP mDNSResponder`  
5. 查看博客的域名解析信息  
`$ dig www.figotan.org +nostats +nocomments +nocmd`

## 配置域名
配置域名纪录的时候需要了解如下一些概念，也需要注意一些问题和坑，避免出现无法访问域名的问题。  

#### 关于Host(主机)  
主机记录就是域名前缀 

| Host(主机)     | 用法             | 
|:-------------:|:----------------------------------:|
| www           | 解析后的域名为 www.figotan.org       |
| @             | 直接解析主域名 figotan.org           |
| *             | 泛解析，匹配其他所有子域名 *.figotan.org |

#### 关于Resord Type(记录类型)  
要指向空间商提供的 IP 地址，选择「类型 A」，要指向一个域名，选择「类型 CNAME」

| Resord Type(记录类型)     | 用法             |  Points To(记录值)|
| :-------------: |:----------------------------------:|:----------------------------------:|
| A(Host)                  | 地址记录，用来指定域名的IPv4地址（如：8.8.8.8），如果需要将域名指向一个IP地址，就需要添加A记录       | 填写您服务器 IP，如果您不知道，请咨询您的空间商|
| AAAA(IPv6 Host)          | 用来指定主机名（或域名）对应的IPv6地址（例如：ff06:0:0:0:0:0:0:c3）记录           | 不常用。解析到 IPv6 的地址|
| CNAME(Alias)             | 如果需要将域名指向另一个域名，再由另一个域名提供ip地址，就需要添加CNAME记录 | 填写空间商给您提供的域名，例如：figotan.org|
| MX(Mail Exchanger)       | 如果需要设置邮箱，让邮箱能收到邮件，就需要添加MX记录| 填写您邮件服务器的IP地址或企业邮局给您提供的域名，如果您不知道，请咨询您的邮件服务提供商|
| TXT(Text)                | 在这里可以填写任何东西，长度限制255。绝大多数的TXT记录是用来做SPF记录（反垃圾邮件）| 一般用于 Google、QQ等企业邮箱的反垃圾邮件设置|
| SRV(Service)             | 记录了哪台计算机提供了哪个服务。格式为：服务的名字、点、协议的类型，例如：_xmpp-server._tcp| 不常用。格式为：优先级、空格、权重、空格、端口、空格、主机名，记录生成后会自动在域名后面补一个“.”，这是正常现象。例如：5 0 5269 xmpp-server.l.google.com.|
| NS(Name Server)          | 域名服务器记录，如果需要把子域名交给其他DNS服务商解析，就需要添加NS记录| 不常用。系统默认添加的两个NS记录请不要修改。NS向下授权，填写dns域名，例如：f1g1ns1.dnspod.net|

#### 关于TTL  
即 Time To Live，缓存的生存时间。指地方dns缓存您域名记录信息的时间，缓存失效后会再次到DNSPod获取记录值。

| TTL     | 用法             | 
|:-------------:|:----------------------------------:|
| 600（10分钟）           | 建议正常情况下使用 600       |
| 60（1分钟）             | 如果您经常修改IP，修改记录一分钟即可生效。长期使用 60，解析速度会略受影响           |
| 3600（1小时）             | 如果您IP极少变动（一年几次），建议选择 3600，解析速度快。如果要修改IP，提前一天改为 60，即可快速生效 |


## 部署内容
之前的博客是放在github，所以首先从github获取博客最新的代码到本地  

#### 第一次从github迁移到双git的流程
详细流程如下：    
1. 从github获取代码  
`git clone git@github.com:figofuture/figofuture.github.io.git`  
2. 进入代码所在的目录  
`cd figofuture.github.io`  
3. 本地master分支跟踪github远程分支  
`git branch --set-upstream-to=origin/master master`  
4. 拉取master最新的代码  
`git pull`  
5. 添加coding.net远程分支  
`git remote add coding-origin git@git.coding.net:figotan/figotan`  
6. 新建本地分支coding-pages  
`git checkout -b coding-pages`  
7. 拉取coding.net远程分支代码到本地coding-pages分支  
`git pull coding-origin coding-pages`  
8. 本地coding-pages分支跟踪远程分支  
`git branch --set-upstream-to=coding-origin/coding-pages coding-pages`  
9. **解决冲突**  
10. 提交本地修改  
`git commit -a -m "xxxxx"`  
11. 推送本地coding-pages到coding远程coding-pages分支  
`git push coding-origin coding-pages`  
12. 切换到master分支  
`git checkout master`  
13. 合并coding-pages分支的修改到master分支  
`git merge --no-ff coding-pages`  
14. 提交master分支到github  
`git push origin master`  

#### 日常编写博客以及同步的流程
以后可以拿其中某一个本地分支(coding-pages)做基准修改，推送到这个本地分支跟踪的远程分支(coding)，然后同步到本地另一个分支(master)，并推送到另一个分支跟踪的远程分支（github），这样，就做到了国内和国外博客内容的同步更新。  
具体的操作流程如下：  
1. 切换到coding-pages分支，并编写博客／上传图片／修改代码  
`git checkout coding-pages`  
2. 提交并推送到coding远程分支  
`git commit -a -m "update"`  
`git push coding-origin coding-pages`  
3. 切换到master分支，合并coding-pages分支的改动到master分支  
`git checkout master`  
`git merge --no-ff coding-pages`  
4. 推送master内容到github,同步完成  
`git push origin master`  

#### 自动化部署方案
如果以上方法你觉得麻烦，其实还有一种自动化的repo同步方案，详见[利用webhook自动同步不同repo之间的代码](https://www.ze3kr.com/2015/08/github-sync-to-gitcafe/)  
这里整理如下：  
1. 注册[openshift](https://www.openshift.com)；  
2. 添加一个应用，根据熟悉的语言选择平台，这里选5.4，设置域名和应用名；  
3. 本地生成ssh公钥私钥，将公钥导入注册的应用；  
4. ssh访问应用；  
5. 在openshift的应用上生成ssh公钥私钥，注意~/.ssh/下无写权限，所以将公钥私钥生成到$OPENSHIFT_DATA_DIR目录下；  
6. 将公钥分别导入到[github](https://github.com)和[coding](https://coding.net)的账户下；  
7. 从coding签出最新的代码  
`cd $OPENSHIFT_DATA_DIR`  
`ssh-agent bash -c 'ssh-add /var/lib/openshift/[openshift分配的名字]/app-root/data/.ssh/id_rsa; git clone git@git.coding.net:figotan/figotan.git'`  
`cd figotan`  
8. 跟踪并获取coding上最新的代码  
`git branch --set-upstream-to=coding-origin/coding-pages coding-pages`  
`ssh-agent bash -c 'ssh-add /var/lib/openshift/[openshift分配的名字]/app-root/data/.ssh/id_rsa; git pull origin coding-pages'`  
9. 添加github远程代码仓库  
`git remote add gh-origin git@github.com:figofuture/figofuture.github.io.git`  
10. 编写php api接口并部署  
`sudo gem install rhc`  
`rhc setup`  
`git clone [申请openshift应用分配的git repo地址]`  
示例参考
  
```php
<?php
if( $_GET['key'] == 'KEY' ) {
    echo shell_exec('cd $OPENSHIFT_DATA_DIR/figotan;ssh-agent bash -c "ssh-add /var/lib/openshift/[openshift分配的名字]/app-root/data/.ssh/id_rsa; git pull origin coding-pages";ssh-agent bash -c "ssh-add /var/lib/openshift/[openshift分配的名字]/app-root/data/.ssh/id_rsa; git push gh-origin coding-pages:master"');
}
else {
    header('HTTP/1.1 400 Bad Request');
    echo <<<HTML
// Fallback
HTML;
}
```
  
`git commit && git push`  
11. 在coding.net项目中设置webhook，记得把**KEY**设置成一串随机的字符串，为了一定的安全性考虑。记得上面步骤中php api源码里的**KEY**和webhook中的**KEY**内容一定要一致。  

## 参考资料
感谢网络上各路英雄的帮助：

[Jekyll自建博客同时托管至 Github 与 Gitcafe](http://www.jianshu.com/p/a96f60d20936)  

[解决百度爬虫无法抓取github pages](http://www.ezlippi.com/blog/2016/02/baidu-spider-forbidden.html)  

[解决 Github Pages 禁止百度爬虫的方法与可行性分析](http://jerryzou.com/posts/feasibility-of-allowing-baiduSpider-for-Github-Pages/)  

[利用 CDN 解决百度爬虫被 Github Pages 拒绝的问题](http://www.dozer.cc/2015/06/github-pages-and-cdn.html)  

[解决 Github Pages 禁止百度爬虫的方法2--从gitcafe迁移到coding.net](http://bblove.me/2016/03/06/migrate-pages-from-gitcafe-to-coding)  

[GitHub 实时同步到 GitCafe，并解决百度无法抓取 GitHub Pages 问题](https://www.ze3kr.com/2015/08/github-sync-to-gitcafe/)  

[知乎上各种解决方案的汇总](http://www.zhihu.com/question/30898326)  