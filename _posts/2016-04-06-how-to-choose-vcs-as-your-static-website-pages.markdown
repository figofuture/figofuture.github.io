---
layout: post
title:  "这些年，我们知道的那些pages平台"
subtitle:   "如何选择代码托管平台托付你的博客"
date:       2016-04-06
author:     "figotan"
header-img: "img/header/toygroup.jpg"
header-mask: 0.5
catalog: true
tags:
    - github
    - gitlab
    - bitbucket
    - sourceforge
    - pages
    - blog
    - 2016
---
>最近看到一则新闻[GitLab向gitlab.com开放了pages服务，而且提供**无限免费**的公有／私有项目空间](https://about.gitlab.com/2016/04/04/gitlab-pages-get-started/)。pages服务大家族又多了一员，那么，对于我们罹患选择困难症的人来说，痛苦多了那么一丢丢。

如何选择，如何选择，如何选择。接下来笔者逐一对比之后，或许可以给你一些启示。那么，我们先来看看目前有哪些比较有名的pages服务吧，看看他们都有哪些cons和prons。

## 知名pages服务有哪些
博客托管是一个很大纵深市场的产品，各种平台层出不穷，从自己的（租用的）硬件服务器，到VPS, IAAS，PAAS, SAAS再到各类空间，直到时下流行的各类代码托管平台提供的pages服务。面对如此多的选择，哪种更适合你，需要先了解下各家各类的优缺良弊，结合自己的条件和所处的环境，在做考量。这里先说下有哪些pages服务。

#### 永远的老大哥sourceforge
[sourceforge.net](http://www.sourceforge.net)或许是做的最早的第三方代码托管平台之一，同时代的还有[Free the Software!](https://sourceware.org)等等，它提供了号称无限免费空间的服务，甚至提供免费的动态网站托管，比如支持PHP，而且提供免费的mysql服务，这样看来，它可以做很多事情了（一个完整的LAMP平台，你猜猜能干啥）。支持基于[用户（开发者）的pages](https://sourceforge.net/p/forge/documentation/Developer%20Web%20Services/)和基于[项目的pages](https://sourceforge.net/p/forge/documentation/Project%20Web%20Services/)。其中，基于项目的pages支持[自定义域名的绑定](https://sourceforge.net/p/forge/documentation/Custom%20VHOSTs/)。缺点是配置比较复杂，上传代码，更新代码较为繁琐，需要用到额外的工具，比如SFTP等。关于sourcefore提供的空间更多请访问[Project Web and Developer Web Server Configuration Details](https://sourceforge.net/p/forge/documentation/Project%20Web%20and%20Developer%20Web/), 我自己上传了一些html,js和flash，可以看看[效果](http://figofuture.users.sourceforge.net/)。  
这几个字永远是那么的有棱有角  
![sourceforge.net](https://images.stackcommerce.com/assets/logo-main-mobile-image/7630/9105f7382c8c1d9d37ebbebe62b9bd26cdce8672_logo_main_mobile.png)  

#### 不赚钱就砍掉算不算作恶googlecode
[google code](https://code.google.com)作为曾经的寄予厚望击败sourcefore的代码托管平台，往事不要再提，现如今，sourceforge还在死亡挣扎，github和gitlab笑看风云，谷歌就像杀掉它无数个兄弟姐妹那样，无情的杀掉了它。  
放个曾经最熟悉的小图标在这里怀念下吧  
![放个曾经最熟悉的小图标在这里怀念下吧](https://code.google.com/images/ph-logo.png)  

闪亮亮的00后哔哩哔哩登场。。。

#### 奇葩的bitbucket
在MAC下撸代码的同志们基本都知道有个图形化git的工具用起来能让你醉生梦死的暂时遗忘了各种git commit branch merge log reset rebase 等等等等难于记忆的命令，这位可以上百晓生兵器谱的[SourceTree](https://www.atlassian.com/software/sourcetree/overview)的老爸[Atlassian](https://www.atlassian.com)有个不太争气的熊儿子叫～叫～叫 [bitbucket](https://bitbucket.org), 我简直不能称呼它为00后，因为[这个](https://confluence.atlassian.com/bitbucket/associate-an-existing-domain-with-an-account-221449746.html)，它叫停了自定义域名绑定业务，甚至连托管起来的pages也不那么美丽，详见我的[页面效果](http://figotan.bitbucket.org/)，你不美丽，自然我的心情也不美丽，所以我觉得你是一朵奇葩。  
好带喜感的标志，左边那个是一只欢快的独眼recycle bin么？
![希望这不是一张遗像](https://d3oaxc4q5k2d6q.cloudfront.net/m/195aa3f25c29/img/homepage/bitbucket-logo-blue.svg)

#### 收费里做的最好的是Assembla是个伪命题
几年前（具体忘了）[Assembla](https://www.assembla.com/)提供免费的无限私有repo，只设置空间上限，貌似是5G，现在新用户已经全线收费，遗憾的是，它并木有提供pages服务，所以列在这里是不合适的，写了这么多，懒得删了，可以跳过看下一个了。  
祝福它发展的越来越好，尽快提供pages服务。  
![](https://upload.wikimedia.org/wikipedia/en/e/e5/Assembla's_logo.png)

#### github还没有笑到最后咧
[github](https://github.com)不多说，撸主们都知道，程序猿的非死不可，撸者尔们的陌陌，贫嘴码呆农的性福生活。 原生支持jekyll, 所有类型的（个人，组织，项目）网页都支持自定义域名，可以按用户／开发者，组织，项目申请pages服务，提交即发布。详见[官方文档](https://help.github.com/categories/github-pages-basics/)，，配置过程简单，基本木有坑。目前领跑代码托管平台，甩小伙伴们不止多少条街了。  
希望八抓鱼一直保持这样的微笑，到最后，没有最后。。。
![](https://assets-cdn.github.com/images/modules/open_graph/github-octocat.png)

#### 国内新贵coding希望不是伪命题
这里简单说下[coding](https://coding.net)，它刚刚完成了对[GitCafe](https://gitcafe.com)的吞食，所以看起来在国内的实力还可以。提供pages服务，支持基于用户和项目的pages,貌似无限免费的私有／公有repo,所有类型页面支持自定义域名，提交即发布。详见[Coding Pages 介绍](https://coding.net/help/doc/pages/index.html)，配置过程简单，基本没有坑。  
祝福所有程序猿都是大师兄  
![我爱编程，我是孙悟空](https://coding.net/static/5ee8025c9dc63a6ff53153705d0e7ce8.png)

#### gitlab王者归来
曾经也不那么牛逼哄哄的[gitorious](https://gitorious.org)伤心太平洋底，墓碑上写着"我会回来的，只不过改了个名字叫**[gitlab](https://gitlab.com)**"，所以，gitlab驾着它的[五彩祥云](http://doc.gitlab.com/ee/pages/README.html)重返中土世界，五王之战即将拉开。说说它的特色吧，首先别人有的它也有，原生支持jekyll, 所有类型的（个人，组织，项目）网页都支持自定义域名，可以按用户／开发者，组织，项目申请pages服务，提交即发布。别人没有的它也有，**原生支持hexo**，没听错吧，噢耶，这意味着你写完文章无需敲那几个该死的命令，hexo generate, hexo deploy，真正的提交即发布。不光原生支持hexo，还支持如下平台：  
* Hugo  
* Middleman  
* Brunch  
* Metalsmith  
* Harp  
* 等等等等，去[这里](https://gitlab.com/groups/pages)看  

最重要的一件事别忘了，也是它的一个坑，**一定要在项目的根目录下写上.gitlab-ci.yml文件，内容根据具体博客平台而异，详细见[官方文档](http://doc.gitlab.com/ee/pages/README.html)**  
想看效果吗，请猛戳[这里](http://figofuture.gitlab.io)  

> "I know everybody."  
> ―Nick Wilde  

![](https://gitlab.com/gitlab-com/gitlab-artwork/raw/master/wordmark/wm_no_bg.png)

## 总结一下
跟着我，左手右手，一个慢动作，画个表总结下我们知道的  

|供应商|免费私有项目|自定义域名|提交即发布|支持PHP|原生支持jekyll|原生支持hexo等|
|:-------:|:--------:|:-------:|:-------:|:-------:|:-------:|:-------:|
|sourceforge|否|只限于项目|否|是|否|否|
| bitbucket |是|否|是|否|否|否|
| github |否|是|是|否|是|否|
|gitlab|是|是|是|否|是|是|
|coding|是|是|是|否|是|否|


## 感谢你们

[Comparison of source code hosting facilities](https://en.wikipedia.org/wiki/Comparison_of_source_code_hosting_facilities)



