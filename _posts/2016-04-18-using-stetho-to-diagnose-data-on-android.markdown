---
layout: post
title:  "如何给安卓APP安装听诊器,检查数据问题"
subtitle:   "开发者在开发中想查看安卓APP运行时的网络访问和数据存储情况，调试太麻烦，日志也挺烦，有没有更好的办法呢？Facebook给广大开发者传了福音，带了福利，放在下午茶的小桌子上，美食干货不敢独吞，所以拿来分享给大家"
date:       2016-04-18
author:     "figotan"
header-img: "img/header/20160418.png"
header-mask: 0.5
catalog: true
tags:
    - Stetho
    - Okhttp
    - Android
    - Facebook
    - Blog
    - 2016
---

从事移动端安卓APP的开发，除了代码逻辑之外就是在和数据打交道。数据的输入输出，往返于网络接口之间，流窜于内存之中存储之内，不能像编写的代码那样直接在代码编辑器中看到其具体的内容。所以如果想窥探数据的真伪对错，目前来说，不外三法。本文开始，告诉你第四条路。

## 现状，以及各自的问题
前面说传统上有两条路可以帮助开发者查看APP运行过程中处理的数据，这里简单描述下处理方式以及每种方式的优缺点。

* 断点调试运行中的APP。你可以用调试器直接调试一个APP，但如果这个APP过于庞大，初始化加载时间很久，那么，最好的调试办法是先将APP在设备（手机或者模拟器）上运行起来，然后用attach to process的方式在被调试的APP进程上加载调试器，这样会比一上来直接调试APP更快一些。想看数据的话，直接在相应代码行加上断点，附着在APP进程上的调试器会自动断下程序，然后查看当前上下文中各种变量的值以及内存的数据，也可以修改这些数据。但是，如果你想看某个数据是不是真的写到存储的文件里，估计需要添加额外的读取代码来查看，而且，每次给APP挂调试器查看数据，感觉还是有些不方便。如果你想诊断和分析网络的访问速度和数据的流量，数据存储的空间和总体数据量，单靠调试这种手段显得力不从心了。而且如果断点的地方在UI的某处代码，长时间处于断点状态查看数据，会导致APP发生**ANR**的异常。
* 加打印日志。类似产品运营的埋点和服务端访问／操作日志，我们也可以在客户端APP相应的位置大书类似**到此一游**和**此地无淫三百靓**的句式，让APP进程通过一个叫**控制台的老东西**（console是计算机世界的老司机了，啥大风大浪没见过的）告诉我们发生了啥，如何发生的，以及发生的结果如何。断点不好做到的网络访问速度和数据流量等东西也可以通过日志叫唤了。这么看起来，貌似加日志已经是一种很完美的办法了。但是，你有没有感觉到这样超级麻烦？首先是你的代码量突然变大了，代码结构变丑了，代码环境卫生变差了，翠花上的酸菜我不敢吃了。相信我，**日志海（骷髅海的代码态）**一定会让你疲惫的双眼犹如狂风暴雨里的一叶孤舟，**说翻就翻**，眼都不带眨一下的。说人话，日志是一种**侵入式**的调试手段，啥叫侵入式？就是它必须由您老人家亲自动手埋藏在代码的心房里,直到天荒地老，APP下架，它也不会化作半点春泥更护花的。而对于调试来说，看日志的情调less than lower，千篇飞过如同嚼蜡。看过安卓日志的童鞋都知道，前尘往事并木有渺云烟，那些个天天在微信群里大呼小叫的群主来看看到底啥叫刷屏。可怜的安卓开发们，天天被日志刷屏。
* 借助第三方工具。对于网络来说，基本就是设置代理，最常用的不外乎**[Charles](http://www.charlesproxy.com)** （收费,基于Java开发，跨平台）；[Fiddler](http://www.telerik.com/fiddler)（免费&收费,基于.Net开发，目前支持通过mono的方式运行在Mac和Linux上）；[Mitmproxy](https://mitmproxy.org)（免费&开源，基于Python开发，跨平台）；还有比较麻烦的办法，比如Http/Https代理＋Wireshark/tcpdump这种。这些工具只能满足网络监控，对于非网络数据就无能为力了。对于存储在手机上的数据，可以通过adb登陆到手机，获得root权限后查看APP内部数据，也可以采用一些安装在手机端的带图形界面的APP来查看和修改数据，比如SQLEditor之类的，这类APP同样需要获取root权限。

那么，后来，[Facebook](https://code.facebook.com)给我们这些可怜的娃带来了福音和福利，试试看咯

## 听诊器来了
[Stetho](http://facebook.github.io/stetho/)英译为“听诊”，是Facebook研发的安卓APP网络诊断和数据监控的框架，目前已经开放[源代码](https://github.com/facebook/stetho)，开发者接入Stetho框架提供的SDK到APP中，这样就可以通过安装在开发机（PC/MAC，Windows/OS X/Linux）上安装的谷歌的[Chrome开发者工具](https://developer.chrome.com/devtools)(通过Chrome浏览器使用)来查看，诊断和分析APP中发生的网络请求和响应以及数据内容，就像用Chrome调试网站一样调试APP程序。当然，几乎任何工具都自带老司机**console**，Stetho也不例外，它提供了一个叫做**dumpapp**的工具，可以向你倾述更多的APP内心世界。  
![](http://www.figotan.org/img/in-post/logo_stetho.png)

## 接入其实很简单
再简单的接入也总有1234步，这里简单叨逼叨逼几句

#### gradle配置
这里不说mvn和low逼的下载&拷贝库的方式了（拷贝源代码的方式集成就更不能忍了），直接上gradle配置

``` gradle
// Gradle dependency on Stetho 
  dependencies { 
    compile 'com.facebook.stetho:stetho:1.3.1' 
  } 
```

如果你使用了Okhttp 3.x的网络栈，请集成如下网络工具库

``` gradle
dependencies { 
    compile 'com.facebook.stetho:stetho-okhttp3:1.3.1' 
  } 
```

Okhttp 2.2.x+

``` gradle
dependencies { 
    compile 'com.facebook.stetho:stetho-okhttp:1.3.1' 
  } 
```

如果使用的是HttpURLConnection

``` gradle
dependencies { 
    compile 'com.facebook.stetho:stetho-urlconnection:1.3.1' 
  } 
```

小白兔和大灰狼请注意：

* 如果你使用的是[Apache HttpClient](http://developer.android.com/intl/zh-cn/about/versions/marshmallow/android-6.0-changes.html#behavior-apache-http-client)，对不起，你out了，请自行升级网络栈，当然你也可以在了解了Stetho的玩法之后自己写一套网络监控来适配Apache HttpClient。
* 如果你使用的网络栈不在上面列举的里面，或者你用c/c++写的网络操作，又或者你采用的协议不是http/https的，那么，网络这部分的诊断和监控方法，估计是很难用了。还想用，自己写咯。

#### 初始化
需要写的代码  
其实很少，而且几乎所有的应用都是一样的代码，首先是在Application类中初始化

``` java
public class MyApplication extends Application {
  public void onCreate() {
    super.onCreate();
    Stetho.initializeWithDefaults(this);
  }
}
```
这个初始化会开启大部分的听诊模块，但是网络监控等一些附加的钩子模块除外

#### 网络诊断
如果你使用的网络栈是OkHttp，而且版本区间在2.2.x+到3.x，那么想要打开网络诊断模块，只需要在程序合适的位置调用如下代码即可

OkHttp 2.2.x+

``` java
OkHttpClient client = new OkHttpClient();
client.networkInterceptors().add(new StethoInterceptor());
```

OkHttp 3.x

``` java
new OkHttpClient.Builder()
    .addNetworkInterceptor(new StethoInterceptor())
    .build();
```

#### HttpURLConnection
如果你使用的HttpURLConnection,稍微有些麻烦的说，你可以使用Stetho框架SDK提供的类StethoURLConnectionManager来完成客户端网络诊断的开启，但是有一些坑是要注意的。
比如为了让Stetho向开发机上的Chrome汇报正确的经过压缩的有效载荷的大小，你需要亲自在http/https的请求头加上**"Accept-Encoding: gzip"**，并且自己处理压缩过的响应数据。如果采用Okhttp，这些都不必劳烦您老人家操心了，框架默认帮你考虑了。
参考代码如下：

``` java
private final StethoURLConnectionManager stethoManager;

private static final int READ_TIMEOUT_MS = 10000;
private static final int CONNECT_TIMEOUT_MS = 15000;

private static final String HEADER_ACCEPT_ENCODING = "Accept-Encoding";
private static final String GZIP_ENCODING = "gzip";

private String url = "http://www.figotan.org";
stethoManager = new StethoURLConnectionManager(url);

URL url = new URL(url);

// Note that this does not actually create a new connection so it is appropriate to
// defer preConnect until after the HttpURLConnection instance is configured.  Do not
// invoke connect, conn.getInputStream, conn.getOutputStream, etc before calling
// preConnect!
HttpURLConnection conn = (HttpURLConnection)url.openConnection();
try {
    conn.setReadTimeout(READ_TIMEOUT_MS);
    conn.setConnectTimeout(CONNECT_TIMEOUT_MS);
    conn.setRequestMethod(request.method.toString());

    // Adding this disables transparent gzip compression so that we can intercept
    // the raw stream and display the correct response body size.
    conn.setRequestProperty(HEADER_ACCEPT_ENCODING, GZIP_ENCODING);

    SimpleRequestEntity requestEntity = null;
    if (request.body != null) {
        requestEntity = new ByteArrayRequestEntity(request.body);
    }

    stethoManager.preConnect(conn, requestEntity);
    try {
          if (request.method == HttpMethod.POST) {
            if (requestEntity == null) {
              throw new IllegalStateException("POST requires an entity");
            }
            conn.setDoOutput(true);
            requestEntity.writeTo(conn.getOutputStream());
          }

          // Ensure that we are connected after this point.  Note that getOutputStream above will
          // also connect and exchange HTTP messages.
          conn.connect();

          stethoManager.postConnect();

    } catch (IOException inner) {
          // This must only be called after preConnect.  Failures before that cannot be
          // represented since the request has not yet begun according to Stetho.
          stethoManager.httpExchangeFailed(inner);
          throw inner;
    }
} catch (IOException outer) {
        conn.disconnect();
        throw outer;
}
      
try {
    ByteArrayOutputStream out = new ByteArrayOutputStream();
    InputStream rawStream = conn.getInputStream();
    try {
        // Let Stetho see the raw, possibly compressed stream.
        rawStream = stethoManager.interpretResponseStream(rawStream);
        if (rawStream != null && GZIP_ENCODING.equals(conn.getContentEncoding())) {
            decompressedStream  = new GZIPInputStream(in);
        } else {
            decompressedStream = rawStream;
        } 
    
        if (decompressedStream != null) {
            int n;
            byte[] buf = new byte[1024];
            while ((n = decompressedStream.read(buf)) != -1) {
                out.write(buf, 0, n);
            }
        }
    } finally {
        if (rawStream != null) {
            rawStream.close();
          }
    }
} finally {
    conn.disconnect();
}

```

通过如上步骤，已经可以让你的APP支持网络监控，数据库监控和SharedPreferences文件内容监控了。如果想玩更高级的，请看后面的自定义**dumpapp**插件和**[Rhino](https://www.mozilla.org/rhino/)**，如果想直接玩起来，请继续往下看。

## 使用起来也不麻烦
使用步骤如下：

1. 首先在手机上运行APP
2. 确保手机USB连接开发机，在开发机上打开Chrome浏览器
3. 在Chrome浏览器地址栏中输入**chrome://inspect**,会看到如下这张图，如果图里面没有你的APP，请返回到上面检查代码接入是否正确  
![](http://www.figotan.org/img/in-post/Snip20160418_1.png)  
4. 点击APP旁边的**inspect**连接，这个时候会弹出一个窗口，如果你用过Chrome的开发者工具，是不是会觉得这个界面很熟悉？对了，这个窗口就是Chrome内置的开发者工具，只不过里面监控的内容从网页变成了APP  
![](http://www.figotan.org/img/in-post/Snip20160418_2.png)  
5. 首先看功能导航条的第一个tab,叫做"Elements"，工作区中的内容是不是很熟悉，[Hierarchy Viewer](http://developer.android.com/tools/performance/hierarchy-viewer/index.html)，很像吧，点击具体的xml节点，可以看到连接的手机上对应的UI控件高亮显示了，这个可以像Hierarchy Viewer那样分析APP页面的嵌套层级  
![](http://www.figotan.org/img/in-post/Snip20160418_3.png)  
![](http://www.figotan.org/img/in-post/Snip20160418_4.png)  
6. 第二个tab叫做"Network",是用来做网络监控的，基本上覆盖了Chrome开发者工具中"Network inspection"的所有功能点，包括下载图片的预览，JSON数据查看，网络请求内容和返回内容  
![](http://www.figotan.org/img/in-post/Snip20160418_13.png)  
7. 第三个tab是"Sources"，用来查看网页的详细内容  
![](http://www.figotan.org/img/in-post/Snip20160418_14.png)  
8. 直接跳过"Timeline", "Profiles"看第六个tab，"Resources"，顾名思义，这里应该就是查看APP内部产生数据的地方啦，目前支持的数据有两种，一种是数据库(ContentProvider和Sql的方式)的数据，另一种是SharedPreferences数据  
![](http://www.figotan.org/img/in-post/Snip20160418_6.png)  
![](http://www.figotan.org/img/in-post/Snip20160418_7.png)  
![](http://www.figotan.org/img/in-post/Snip20160418_8.png)  
![](http://www.figotan.org/img/in-post/Snip20160418_9.png)  
![](http://www.figotan.org/img/in-post/Snip20160418_11.png)  
![](http://www.figotan.org/img/in-post/Snip20160418_16.png)  

9. "Audits" 跳过，如同"Timeline"和"Profiles"，目前没怎么支持，有待进一步发掘的功能。
10. "Console"老司机下面讲

## 有坑吗？
关于网络监控的有一些需要注意的字段的含义，详细的内容可以去精读一遍[Chrome开发者工具官方文档](https://developer.chrome.com/devtools/docs/network)  
![](http://www.figotan.org/img/in-post/Snip20160418_15.png)  
这里只详细解释下上面图中个字段的含义。

* Name/Path    网络资源的名称和URL路径，比如http://www.figotan.org/c/v/logo.jpg这个网络资源，Name是logo.jpg Path是www.figotan.org/c/v/
* Method    HTTP协议规定的请求方法，比如GET POST
* Status/Text    HTTP协议规定的返回码和这个返回码对应的含义解释文字 ，比如200/OK
* Type    请求资源的MIME类型，比如application/json image/jpeg image/png等等
* Initiator    发起请求的对象，可以是Parser/Redirect/Script/Other,详见上面的官方文档
* Size/Content    Size表示HTTP响应的头和数据体的和，由远程服务端返回；Content是返回的资源解码后的大小
* Time/Latentcy    Time是总的时间间隔，从发起请求开始到接收到服务端返回的最后一个字节为止；Latency是指的接收到服务端返回的第一个字节消耗的时间
* Timeline    显示了所有网络请求的瀑布流

## 还可以做什么
除了监控网络，查看／修改数据之外，还可以做很多事情，因为Stetho预留了两种接口，为了可持续的发展

#### 自定义dumpapp插件
自定义插件是让老司机dumpapp get新技能的首选方法，可以很容易地在配置过程中添加。只需如下代码即可添加自定义插件：

``` java
Stetho.initialize(Stetho.newInitializerBuilder(context)
        .enableDumpapp(new DumperPluginsProvider() {
          @Override
          public Iterable<DumperPlugin> get() {
            return new Stetho.DefaultDumperPluginsBuilder(context)
                .provide(new HelloWorldDumperPlugin())
                .provide(new APODDumperPlugin(context.getContentResolver()))
                .finish();
          }
        })
        .enableWebKitInspector(new ExtInspectorModulesProvider(context))
        .build());
```

其中HelloWorldDumperPlugin和APODDumperPlugin是自定义的插件，具体内容可以参考[Stetho提供的sample程序](https://github.com/facebook/stetho/tree/master/stetho-sample)  
执行dumpapp命令需要先从git取下最新的代码，然后找到dumpapp脚本，并执行

``` bash
$ git clone https://github.com/facebook/stetho.git
$ cd stetho
// 列举出支持的命令（插件）
$ ./scripts/dumpapp -p com.facebook.stetho.sample -l
```

参照sample代码编写dumpapp插件，然后用dumpapp命令验证插件的效果

#### Stetho对于JavaScript的支持
目前Stetho对于JavaScript脚本的支持是采用内嵌Mozilla研发的**[Rhino](https://www.mozilla.org/rhino/)**  
第一种采用dumpapp的插件扩展方式虽然功能强大，无所不能，但是完成一件事情需要一定的技术和时间成本，必须经历一系列的编写，编译，构建，安装，调试，修改代码，再下一个轮回，迭代几次后才能形成产出，这其实是类c/c++/java这种非动态语言的一个缺陷，软件的研发周期太长。那么，如果有一种写完即发布的脚本语言能够支持起来，其实对于研发效率来说，是有很大提升的，比如lua/javascript/perl/python/groovy等等，这样的语言轻巧，无需编译，写完就可以发布验证，甚至可以边写边调试边上线。  
Chrome开发者工具原生支持JavaScript，所以Stetho也提供了JavaScript的支持。

Rhino是一个可以运行在Java程序内部的JavaScript实现，由Mozilla开发并发布为一个[开源的项目](https://github.com/mozilla/rhino)
![](http://www.figotan.org/img/in-post/68747470733a2f2f646576656c6f7065722e6d6f7a696c6c612e6f72672f406170692f64656b692f66696c65732f3833322f3d5268696e6f2e6a7067.jpeg)  

下面说说集成和使用方式

如果要让APP支持Rhino, 首先是gradle配置  

``` gradle
dependencies { 
    compile 'com.facebook.stetho:stetho-js-rhino:1.3.1' 
} 
```

然后就可以通过开发机上Chrome浏览器提供的开发者工具的"Console"老司机（任何工具都有老司机console）来发射JavaScript代码了，参考代码如下：

``` javascript
importPackage(android.widget);
importPackage(android.os);
var handler = new Handler(Looper.getMainLooper());
handler.post(function() { Toast.makeText(context, "Hello from JavaScript", Toast.LENGTH_LONG).show() });
```

运行效果如下：  
![](http://www.figotan.org/img/in-post/Snip20160419_17.png)  

如果你想通过APP传递一些变量，类，闭包和函数给JavaScript运行环境中，那么你可以在Stetho初始化的时候添加如下代码:  

``` java
Stetho.initialize(Stetho.newInitializerBuilder(context)
        .enableWebKitInspector(new InspectorModulesProvider() {
          @Override
          public Iterable<ChromeDevtoolsDomain> get() {
            return new DefaultInspectorModulesBuilder(context).runtimeRepl(
                new JsRuntimeReplFactoryBuilder(context)
                    // Pass to JavaScript: var foo = "bar";
                    .addVariable("foo", "bar")
                    .build()
            ).finish();
          }
        })
        .build());
```

更多玩法请移步[Rhino on Stetho](https://github.com/facebook/stetho/tree/master/stetho-js-rhino)

## 参考资料
[Stetho官方文档](http://facebook.github.io/stetho/)  
[Stetho源代码](https://github.com/facebook/stetho)  
[Stetho: A new debugging platform for Android](https://code.facebook.com/posts/393927910787513/stetho-a-new-debugging-platform-for-android/)  
[A First Glance at Stetho tool](http://gmariotti.blogspot.it/2015/07/a-first-glance-at-stetho-tool.html)  
[Remote Debugging on Android with Chrome](https://developer.chrome.com/devtools/docs/remote-debugging)  
[Chrome DevTools Overview](https://developer.chrome.com/devtools)  

