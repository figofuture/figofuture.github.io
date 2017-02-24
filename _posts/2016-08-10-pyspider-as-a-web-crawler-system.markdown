---
layout: post
title:  "漫谈Pyspider网络爬虫的实践"
subtitle:   ""
date:       2016-08-10
author:     "figotan"
header-img: "img/header/20160810.jpg"
header-mask: 0.5
catalog: true
tags:
    - 网络爬虫
    - Pyspider
    - Python
    - 2016
---

感觉很久没有写点东西了，因为最近太忙（外因）或是自身太懒（内因）的原因。总之，很早之前，我就开始规划着写点关于网络爬虫方面的文章，介绍性质的，但更重要的是，计算机以及信息科学的实践性，所以，以一个实干者的角度来写，更为合适一些。  
在这之前，还是有必要对一些概念性的词汇做一下梳理和科普，至少，不会让读者觉得突兀或者一知半解的读着流水账式的文字。

## 什么是网络爬虫  
来一段靠谱的**维基百科**的权威解释  

> 网络蜘蛛（Web spider）也叫网络爬虫（Web crawler），蚂蚁（ant），自动检索工具（automatic indexer），或者（在FOAF软件概念中）网络疾走（WEB scutter），是一种“自动化浏览网络”的程序，或者说是一种网络机器人。它们被广泛用于互联网搜索引擎或其他类似网站，以获取或更新这些网站的内容和检索方式。它们可以自动采集所有其能够访问到的页面内容，以供搜索引擎做进一步处理（分检整理下载的页面），而使得用户能更快的检索到他们需要的信息。  

所以网络爬虫是一种自动化运行的获取目标网页信息内容并按一定结构存储的便于查询检索的计算机程序。

这段话似乎让人看了很晕很懵逼，这个什么鬼的到底能做啥呢？  
举几个很通俗的例子吧，比如你一手掌握了很多看美女图片和小电影的网站和论坛（这里都指的线上的，网络上的资源），每当夜深人静的时候，你偷偷打开电脑，秒开浏览器那个只有你本人知道的收藏夹，然后一顿点看撸晃，但是，等等，其实真实的体验没这么流畅？

* 信息太散了，每个资源都需要单独去访问，浪费时间的点击
* 资源良莠不齐，口味繁杂，需要花时间找到自己喜欢的内容
* 大量烦人的广告，这尼玛是广告还是马赛克？还是播放？还是下载？
* 手动找更新，又是一件繁重的工作

所以想看更纯粹更专注的内容，自己动手，写爬虫吧。  

你说我没这需求，那没关系，你总需要看新闻吧，看文学吧，逛电商吧，看股票吧，总有那么一类信息，你会关注的，但是现有的服务总是不够单纯，界面不够友好，信息不够纯粹，所以，这是你需要学习并实践网络爬虫技术的动机。

## 为什么是Python  
写网络爬虫的语言有很多，编程的语言更多。个人认为Python是一种工具型的语言，上手快，语法简单（相比于C/C++/JAVA族），各种功能库丰富而且小巧单一（每个独立的库只做一件事情），所以编程就像是在玩乐高积木，照着自己设计好的流程，拼接就行了。当然，这是笔者个人的经验和喜好。如果你有自己擅长并喜欢的，大可用自己的去实现一个网络爬虫系统，这个不在本文的讨论范围之类了。  
有关几种编程语言编写网络爬虫的比较，可以参考知乎上的文章 [PHP, Python, Node.js 哪个比较适合写爬虫？](https://www.zhihu.com/question/23643061)

## 为什么是Pyspider  
Python有很多成熟的网络爬虫框架， 知乎上很多大牛总结了一些实践经验，具体可以参考[如何入门 Python 爬虫？](https://www.zhihu.com/question/20899988)  
很多推荐用requests做请求，query／soup做页面数据(Html/Xml)解析，看起来很灵活，然而，一个比较完善的网络爬虫系统，所需要提供的功能可能远远不止这些。也有推荐Scrapy的，虽然看起来功能非常强大，但是这个框架上手需要一些时间，有一定的学习成本，相对于新手来说，很难快速专注爬虫业务的开发。  
Pyspider是[Roy Binux](https://github.com/binux)开发的一款开源的网络爬虫系统，它不止是一个爬虫框架，而是一套完备的爬虫系统，使用这套系统你只需要关注两件事情

* 目标网站上的内容元素的解析，而且只需要关注解析什么，解析框架也有提供，并且提供了可视化工具辅助从目标页面抠取需要解析的元素CSS属性
* 解析出来的内容元素如何保存，你只需要关注数据库表字段的设计，然后把解析出来的页面元素内容保存到数据库表中
* 那么，剩下的几乎所有事情，就交给Pyspider吧

是不是听上去感觉很简单，那么，开始动手吧，跟着这篇[官方文档](http://docs.pyspider.org/en/latest/)，最快几分钟的功夫，你就可以学会从2048（草榴）找到真爱了。

简单的爬取看官方文档就可以了，不过，实践过程中总会遇到各种问题，那么，看看这些如何解决的吧。

## 如何模拟登陆  
有些网站内容的展示需要用户登录，那么如果需要爬取这样的页面内容，我们的爬虫就需要模拟用户登陆。网站一般在页面跳转或者刷新的时候，也需要获取登录信息以确定这个页面的访问用户是登陆过的。如果每次都需要用户重新登录，那么这种体验就太烂了，需要一种机制把之前用户登陆的信息保存起来，而且一定是保存在浏览器可以访问的本地存储上，这样，用户在页面跳转或者页面刷新的时候，登录信息被网站自动读取，就不需要用户频繁登录了。而这个保存的地方，叫做Cookie。  
爬虫需要做的事情，一是模拟登陆，拿到Cookie数据，然后保存下来，二是每次去访问网页的时候，将Cookie信息传递给请求，这样就可以正常爬到需要用户登录的数据了。

我们先设计一个登录类，用来管理登录的请求和数据

```
import urllib
import urllib2
import lxml.html as HTML

class Login(object):

    def __init__(self, username, password, login_url, post_url_prefix):
        self.username = username
        self.password = password
        self.login_url = login_url
        self.post_url_prefix = post_url_prefix

    def login(self):
        post_url, post_data = self.getPostData()
        post_url = self.post_url_prefix + post_url
        req = urllib2.Request(url = post_url, data = post_data)
        resp = urllib2.urlopen(req)
        return True

    def getPostData(self):
        url = self.login_url.strip()
        if not re.match(r'^http://', url):
            return None, None
        req = urllib2.Request(url)
        resp = urllib2.urlopen(req)
        login_page = resp.read()
        doc = HTML.fromstring (login_page)
        post_url = doc.xpath("//form[@method='post' and @id='lsform']/@action")[0]
        cookietime = doc.xpath("//input[@name='cookietime' and @id='ls_cookietime']/@value")[0]
        username = self.username
        password = self.password
        post_data = urllib.urlencode({
            'fastloginfield' : 'username',
            'username' : username,
            'password' : password,
            'quickforward' : 'no',
            'handlekey' : 'ls',
            'cookietime' : cookietime,
        })
        return post_url, post_data
```

代码解释  

* 用户名username, 密码password, 目标网站的登录页面地址login_url, 目标网站的主域名post_url_prefix，这些参数从外部传入，目标网站的登录页面地址也有可能就是网站的主页地址。
* getPostData首先向目标网站的登录页面地址发起一个请求，然后解析这个页面的数据，解析出登录请求的目标地址和post请求的数据（登录请求一般为post请求），然后返回这两个参数

设计一个方法，这个方法用来获取爬取网页请求需要的Cookie数据。

```
import os
import hashlib
import cookielib

LOGIN_URL = 'http://登录页面地址'
USER_NAME = '用户名'
PASSWORD = '密码'

HOST = '目标网页主域名'
REFERER = 'http://目标网页主域名/'
POST_URL_PREFIX = 'http://目标网页主域名/'

# !!! Notice !!!
# Tasks that share the same account MUST share the same cookies file
COOKIES_FILE = '/tmp/pyspider.%s.%s.cookies' % (HOST, hashlib.md5(USER_NAME).hexdigest())
COOKIES_DOMAIN = HOST

USER_AGENT = 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/42.0.2311.135 Safari/537.36'
HTTP_HEADERS = {
    'Accept' : 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8',
    'Accept-Encoding' : 'gzip, deflate, sdch',
    'Accept-Language' : 'zh-CN,zh;q=0.8,en;q=0.6',
    'Connection' : 'keep-alive',
    'DNT' : '1',
    'Host' : HOST,
    'Referer' : REFERER,
    'User-Agent' : USER_AGENT,
}

def getCookies():
    cookiesJar = cookielib.MozillaCookieJar(COOKIES_FILE)
    if not os.path.isfile(COOKIES_FILE):
        cookiesJar.save()
    cookiesJar.load (COOKIES_FILE)
    cookieProcessor = urllib2.HTTPCookieProcessor(cookiesJar)
    cookieOpener = urllib2.build_opener(cookieProcessor, urllib2.HTTPHandler)
    for item in HTTP_HEADERS:
        cookieOpener.addheaders.append ((item ,HTTP_HEADERS[item]))
    urllib2.install_opener(cookieOpener)
    if len(cookiesJar) == 0:
        login = Login(USER_NAME, PASSWORD, LOGIN_URL, POST_URL_PREFIX)
        if login.login():
            cookiesJar.save()
        else:
            return None
    cookiesDict = {}
    for cookie in cookiesJar:
        if COOKIES_DOMAIN in cookie.domain:
            cookiesDict[cookie.name] = cookie.value
    return cookiesDict
```

代码解释

* USER_NAME PASSWORD LOGIN_URL POST_URL_PREFIX 分别定义了用户名／密码／登录页面地址／目标网页前缀
* 如果从COOKIES_FILE读取出的Cookie信息为空，那么就调用Login做登录流程，并且把获取到的结果保存，如果Cookie不为空，就返回Cookie信息到字典cookiesDict中

Pyspider每次爬取请求都带上Cookie字典，这样，向目标地址发请求就可以获取到需要登录才能访问到的数据了。

```
cookies = getCookies()
self.crawl(url, cookies = cookies, callback=self.index_page)
```

## 如何解析爬取下来的内容  
爬取的内容通过回调的参数**response**返回，**response**有多种解析方式  

* 如果返回的数据是json，则可以通过**response.json**访问
* **response.doc**返回的是**PyQuery**对象
* **response.etree**返回的是**lxml**对象
* **response.text**返回的是**unicode**文本
* **response.content**返回的是**字节码**

所以返回数据可以是5种形式，unicode和字节码不是结构化的数据，很难解析，这里就不赘述了，json需要特定的条件，而且解析相对简单，也不必说。  
常用的就是PyQuery和lxml的方式，关于lxml，可以采用XPath的语法来解析，比如前面模拟登录中就采用了xpath的语法解析网页，具体可参考lxml和XPath的[相关文档](http://www.w3schools.com/xsl/xpath_syntax.asp)。  

XPath选择器参考

|选择器|示例|示例说明|
|:-------:|:-------|:-------|
|nodename|bookstore|选择所有名称叫做"bookstore"的节点|
|/|bookstore/book|选择"bookstore"的节点的所有"book"子节点|
|//|//book|选择文档中所有名称叫做"book"的节点，不管它们的父节点叫做什么|
|.||选择当前的节点|
|..||选择当前节点的父节点|
|@|//@lang|选择所有名称叫做"lang"的属性|
||bookstore//book|选择节点"bookstore"所有叫做"book"的子孙节点，bookstore不一定是book的父节点|
||/bookstore/book[1]|选择节点"bookstore"的第一个叫做"book"的子节点|
||/bookstore/book[last()]|选择节点"bookstore"的最后一个叫做"book"的子节点|
||//title[@lang]|选择所有有一个属性名叫做"lang"的title节点|
||//title[@lang='en']|选择所有有一个属性"lang"的值为"en"的title节点|
|*|/bookstore/*|选择"bookstore"节点的所有子节点|
||//*|选择文档中所有的节点|
|@*|//title[@*]|选择所有的"title"节点至少含有一个属性，属性名称不限|

PyQuery可以采用**CSS选择器**作为参数对网页进行解析。  
类似这样

```
response.doc('.ml.mlt.mtw.cl > li').items()
```

或者这样

```
response.doc('.pti > .pdbt > .authi > em > span').attr('title')
```

关于PyQuery更多玩法，可以参考[PyQuery complete API](https://pythonhosted.org/pyquery/api.html)  

CSS选择器  

|选择器|示例|示例说明|
|:-------:|:-------|:-------|
|.class|.intro|Selects all elements with class="intro"|
|#id|#firstname|Selects the element with id="firstname"|
|element|p|Selects all \<p\> elements|
|element,element|div, p|Selects all <div> elements and all <p> elements|
|element element|div p|Selects all \<p\> elements inside \<div\> elements|
|element>element|div > p|Selects all \<p\> elements where the parent is a \<div\> element|
|[attribute]|[target]|Selects all elements with a target attribute|
|[attribute=value]|[target=_blank]|Selects all elements with target="_blank"|
|[attribute^=value]|a[href^="https"]|Selects every \<a\> element whose href attribute value begins with "https"|
|[attribute$=value]|a[href$=".pdf"]|Selects every \<a\> element whose href attribute value ends with ".pdf"|
|[attribute*=value]|a[href*="w3schools"]|Selects every \<a\> element whose href attribute value contains the substring "w3schools"|
|:checked|input:checked|Selects every checked \<input\> element|

更多详情请参考[CSS Selector Reference](http://www.w3schools.com/cssref/css_selectors.asp)

## 如何将数据保存到MySQL中  
将MySQL的数据库访问封装成一个类

```
import hashlib
import unicodedata
import mysql.connector
from mysql.connector import errorcode

class MySQLDB:
    
    username = '数据库用户名'
    password = '数据库密码'
    database = '数据库名'
    host = 'localhost'    #数据库主机地址
    connection = ''
    isconnect = True
    placeholder = '%s'
    
    def __init__(self):
        if self.isconnect:
            MySQLDB.connect(self)
            MySQLDB.initdb(self)
    
    def escape(self,string):
        return '`%s`' % string
    
    def connect(self):
        config = {
            'user':self.username,
            'password':self.password,
            'host':self.host
        }
        
        if self.database != None:
            config['database'] = self.database
    
        try:
            cnx = mysql.connector.connect(**config)
            self.connection = cnx
            return True
        except mysql.connector.Error as err:
            if (err.errno == errorcode.ER_ACCESS_DENIED_ERROR):
                print "The credentials you provided are not correct."
            elif (err.errno == errorcode.ER_BAD_DB_ERROR):
                print "The database you provided does not exist."
            else:
                print "Something went wrong: " , err
            return False
        
    def initdb(self):
        if self.connection == '':
            print "Please connect first"
            return False
        cursor = self.connection.cursor()
        
        # 创建表的定义
        sql = 'CREATE TABLE IF NOT EXISTS \
        table_name ( \
        id VARCHAR(64) PRIMARY KEY, \
        url TEXT, \
        title TEXT, \
        type TEXT, \
        thumb TEXT, \
        count INTEGER, \
        images TEXT, \
        tags TEXT, \
        post_time DATETIME \
        ) ENGINE=INNODB DEFAULT CHARSET=UTF8'
        try:
            cursor.execute(sql)
            self.connection.commit()
            return True
        except mysql.connector.Error as err:
            print ("An error occured: {}".format(err))
            return False
        
    def cleardb (self):
        if self.connection == '':
            print "Please connect first"
            return False
        cursor = self.connection.cursor()
        sql = 'DROP TABLE IF EXISTS table_name'
        try:
            cursor.execute(sql)
            self.connection.commit()
            return True
        except mysql.connector.Error as err:
            print ("An error occured: {}".format(err))
            return False
        
    def insert (self,**values):
        if self.connection == '':
            print "Please connect first"
            return False
        cursor = self.connection.cursor()
        # 插入数据
        sql = "insert into table_name (id, url, title, type, thumb, count, temperature, images, tags, post_time) values (%s,%s,%s,%s,%s,%s,%s,%s,%s) on duplicate key update id=VALUES(id), url=VALUES(url), title=VALUES(title), type=VALUES(type), thumb=VALUES(thumb), count=VALUES(count), images=VALUES(images), tags=VALUES(tags), post_time=VALUES(post_time)"
        title = unicodedata.normalize('NFKD', values['title']).encode('ascii','ignore')
        images = ", ".join('%s' % k for k in values['images'])
        params = (hashlib.md5(title + images).hexdigest(), values['url'], values['title'], values['type'], values['thumb'], values['count'], images, '', values['date'])
        try:
            cursor.execute(sql,params)
            self.connection.commit()
            return True
        except mysql.connector.Error as err:
            print ("An error occured: {}".format(err))
            return False
    
    def replace(self,tablename=None,**values):
        if self.connection == '':
            print "Please connect first"
            return False
    
        tablename = self.escape(tablename)
        if values:
            _keys = ", ".join(self.escape(k) for k in values)
            _values = ", ".join([self.placeholder, ] * len(values))
            sql_query = "REPLACE INTO %s (%s) VALUES (%s)" % (tablename, _keys, _values)
        else:
            sql_query = "REPLACE INTO %s DEFAULT VALUES" % tablename
                
        cur = self.connection.cursor()
        try:
            if values:
                cur.execute(sql_query, list(itervalues(values)))
            else:
                cur.execute(sql_query)
            self.connection.commit()
            return True
        except mysql.connector.Error as err:
            print ("An error occured: {}".format(err))
            return False
```

在处理爬取结果的回调中保存到数据库

```
def on_result(self, result):
        db = MySQLDB()
        db.insert(**result)
```

## 如何在爬虫脚本更新后重新运行之前执行过的任务  
比如这种场景，爬取了一些数据，发现没有写保存到数据库的逻辑，然后加上了这段逻辑，却发现之前跑过的任务不会在执行了。那么如何做到在爬虫脚本改动后，之前的任务重新自动再跑一遍呢。  
在**crawl_config**中使用**itag**来标示爬虫脚本的版本号，如果这个值发生改变，那么所有的任务都会重新再跑一遍。示例代码如下

```
class Handler(BaseHandler):
    crawl_config = {
        'headers': {
            'User-Agent': USER_AGENT,
        },
        'itag': 'v1'
    }
```

**itag**也可以用来控制特定的任务是否需要重新执行，详见[官方文档](http://docs.pyspider.org/en/latest/apis/self.crawl/#itag)。

## 如何解析JavaScript代码
具体如何使用的可以看官方文档，这里列举出一些可供参考的JavaScript解析器  
基于**Webkit**的[PhantomJS](http://phantomjs.org/)
基于**Gecko**的[SlimerJS](http://slimerjs.org/)  
基于**PhantomJS**和**SlimerJS**的[CasperJS](http://casperjs.org/)  
[Nightmare](http://www.nightmarejs.org/)  
[Selenium](http://www.seleniumhq.org/)  
[spynner](https://github.com/makinacorpus/spynner)  
[ghost.py](https://github.com/jeanphix/Ghost.py)  

更多工具／框架请参考[Headless Browser and scraping - solutions](http://stackoverflow.com/questions/18539491/headless-browser-and-scraping-solutions)

## 参考资料  

[binux/pyspider](https://github.com/binux/pyspider)  
[Pyspider官方文档](http://docs.pyspider.org/en/latest/)  
[pyspider架构设计](http://blog.binux.me/2014/02/pyspider-architecture/)  
[pyspider中文脚本编写指南](http://www.jishubu.net/yunwei/python/411.html)  
[Pyspider爬虫教程](http://www.cnblogs.com/panliu/p/4524157.html)  
[把 pyspider的结果存入自定义的mysql数据库中](http://www.jishubu.net/yunwei/python/424.html)  
[pyspider的mysql数据存储接口](http://blog.csdn.net/u012293522/article/details/44222207)  
[PyQuery complete API](https://pythonhosted.org/pyquery/api.html)  
[CSS Selector Reference](http://www.w3schools.com/cssref/css_selectors.asp)  

## 收集的一些其它网络爬虫的资料  

#### Java
[雪球股票信息超级爬虫](https://github.com/decaywood/XueQiuSuperSpider)  
[一个简单易用的爬虫框架,内置代理管理模块,灵活设置多线程爬取](https://github.com/MrJiao/SpiderJackson)  
[A scalable web crawler framework for Java](https://github.com/code4craft/webmagic)  
[强力 Java 爬虫，列表分页、详细页分页、ajax、微内核高扩展、配置灵活](https://git.oschina.net/l-weiwei/Spiderman2)  

#### Python
[Scrapy](http://scrapy.org/)  
[a smart stream-like crawler & etl python library](https://github.com/ferventdesert/etlpy)  
[爬视频音频神器You-Get](https://you-get.org/)  
[另一款视频下载神器youtube-dl](https://github.com/rg3/youtube-dl)  
[youtube-dl图形界面版](https://github.com/MrS0m30n3/youtube-dl-gui)  
[自动抓取Tumblr指定用户视频分享](https://github.com/Thoxvi/MyCar_python)  
[crawley](https://github.com/jmg/crawley)  
[乌云公开漏洞、知识库爬虫和搜索](https://github.com/hanc00l/wooyun_public)  
[下载指定的 Tumblr 博客中的图片，视频](https://github.com/dixudx/tumblr-crawler)  
[下载指定的 Tumblr 博客中的图片，视频，玄魂修改版](https://github.com/xuanhun/tumblr-crawler)  
[DHT网络爬虫](https://github.com/78/ssbc)  
[豆瓣电影、书籍、小组、相册、东西等爬虫集 writen in Python](https://github.com/dontcontactme/doubanspiders)  
[如何不用客户端下载 YouKu 视频-YouKu 实现下载 Python3 实现](http://zpz.name/2378/)  

#### PHP
[PHP Crawler](https://sourceforge.net/projects/php-crawler/)  
[PHPCrawl](https://sourceforge.net/projects/phpcrawl/)  
[Phpfetcher](https://github.com/fanfank/phpfetcher)  
[php spider framework](https://github.com/hirudy/Tspider)  
[我用爬虫一天时间“偷了”知乎一百万用户，只为证明PHP是世界上最好的语言](https://github.com/owner888/phpspider)  

#### Nodejs
[Nodejs 编写的爬虫工具](https://github.com/xwartz/spider)  
[批量抓取AV磁链或封面的苦劳力](https://github.com/raawaa/jav-scrapy)  
[Easily download all the photos from a Tumblr blog.](https://github.com/Leeiio/tumblr-downloader)  
[DHT Spider + BitTorrent Client = P2P Spider](https://github.com/dontcontactme/p2pspider)  

#### Ruby
[A simple DHT crawler, written in Ruby](https://github.com/dontcontactme/rbdht)  

#### C sharp
[visualized crawler & ETL IDE written with C#/WPF](https://github.com/ferventdesert/Hawk)  

#### Erlang
[使用erlang实现P2P磁力搜索](https://github.com/kevinlynx/dhtcrawler2)  

#### C++

[给不了你梦中情人，至少还有硬盘女神：hardseed](https://github.com/yangyangwithgnu/hardseed)  