---
layout: post
title:  "安卓Secret Codes大全"
subtitle:   "使用安卓Secret Codes可以开启一些系统以及应用的隐藏功能"
date:       2016-06-15
author:     "figotan"
header-img: "img/header/20160615.jpg"
header-mask: 0.3
catalog: true
published: true
tags:
    - Android
    - MMI
    - USSD
    - 2016
---

安卓系统除了慢，卡顿，耗电，耗流量，体验千奇百怪，漏洞多且不安全不靠谱之外，还有很多稀奇古怪的玩意儿。比如利用漏洞完成某些看起来不可能实现的任务所采用的黑魔法。当然，也有本文说的蓝魔法，Secret Codes.

## 什么是Secret Codes
一个系统或者应用，提供了一些高阶的功能（必杀技，大招），但是功能的开发者又不想让所有的用户都知道并且轻而易举的获取并使用这些功能（往往都是些很强大的功能，但是危险系数也很高，比如可以擦除系统上所有的数据，又或者能看到内幕信息，比如系统底层的运行数据）。那么，将这些功能的界面入口隐藏起来，并且通过一个**咒语**来开启和关闭，这样的**蓝魔法**就是**Secret Codes**。  
不管是**蓝魔法**还是**Secret Codes**，这些都是方便非技术小白用户理解的通俗叫法。其实，在移动通讯领域，它的学名叫做**USSD**或者**MMI codes**，那么，这些又是啥，下面简单解释下，如果对太专业的东西没兴趣，可以直接跳过看下一节。

#### 什么是USSD  
**USSD**（Unstructured Supplementary Service Data）全称非结构化补充数据业务，是一种基于GSM网络的交互式数据业务。当你使用手机键盘输入一些网络已预先制定的数字或者符号比如*#等，再按send也就是拨号键就可以向网络发送一条指令，网络根据你的指令选择你需要的服务提供给你。  
来自[维基百科](https://en.wikipedia.org/wiki/Unstructured_Supplementary_Service_Data)的解释  
> Unstructured Supplementary Service Data (USSD), sometimes referred to as "Quick Codes" or "Feature codes", is a protocol used by GSM cellular telephones to communicate with the service provider's computers. USSD can be used for WAP browsing, prepaid callback service, mobile-money services, location-based content services, menu-based information services, and as part of configuring the phone on the network.

#### 什么是MMI  
MMI(Man Machine Interface)全称 人机界面，所以这些codes就是人机界面交互的指令编码，它和USSD有什么不同，[这篇文章](https://berlin.ccc.de/~tobias/mmi-ussd-ss-codes-explained.html)很好的解释了一切，这里不在赘述。

#### Android系统中关于这部分的源代码  
CDMA  
https://android.googlesource.com/platform/frameworks/opt/telephony/+/android-7.1.1_r4/src/java/com/android/internal/telephony/cdma/CdmaMmiCode.java  
GSM  
https://android.googlesource.com/platform/frameworks/opt/telephony/+/android-7.1.1_r4/src/java/com/android/internal/telephony/gsm/GsmMmiCode.java  

## 蓝魔法能做什么
如果我们念完咒语，看看它能做些什么

* 清除手机所有的设置并恢复到第一次开机的状态
* 清空手机里所有的数据（安装的APP，设置，保存的密码，照片，音频，视频，下载的文件，等等）并重启手机
* 重新安装手机的操作系统
* 屏幕等硬件功能检测
* 获取产品代码
* 查看电池状态
* 查看手机状态
* 查看网络状态
* 更多

## 咒语大全
咒语一览表

#### 通用功能

|代码|功能描述|备注|
|:-------:|:--------:|:-------:|
|\*\#\*\#7780\#\*\#\*|恢复手机出厂设置，清除应用数据以及设置，移除手机绑定的谷歌账号，卸载下载的应用|不会删除系统内置应用，不会删除SD Card上的文件|
|\*2767\*3855\#|重新安装手机操作系统，恢复手机出厂状态，删除包括安装在内部存储上的所有APP并清除设置||
|\*\#\*\#197328640\#\*\#\*|进入调试模式||
|\*\#\*\#4636\#\*\#\*|电话基本信息，电话使用情况，电池信息||
|\*\#\*\#34971539\#\*\#\*|摄像头系统信息|注意不要点击不要点击升级摄像头系统信息，小心1秒变砖|
|\*\#\*\#7594\#\*\#\*|改变电源按键的功能，允许直接关机而不是询问用户选择操作（无声模式，飞行模式，关机）||
|\*\#\*\#273283\*255\*663282\*\#\*\#\*|备份所有的媒体文件，打开文件拷贝界面让你能够备份图片，视屏和音频等媒体文件||
|\*\#\*\#8255\#\*\#\*|启动GTalk服务监控||
|\*2767\*4387264636\*|显示产品信息||
|\*\#0228\#|显示电池状态||
|\*\#12580\*369\*|软件和硬件信息||
|\*\#32489\#|查看加密信息||
|\*\#273283\*255*3282\*\#|数据创建菜单||
|\*\#3282\*727336\*\#|数据使用状态||
|\*\#8736364\#|OTA升级菜单||
|\#\#778|显示EPST菜单||
|\*\#228\#|ADC读取菜单||

#### WIFI,GPS和蓝牙检测

|代码|功能描述|备注|
|:-------:|:--------:|:-------:|
|\*\#\*\#526\#\*\#\*|WLAN检测||
|\*\#\*\#528\#\*\#\*|WLAN测试||
|\*\#\*\#232339\#\*\#\*|WLAN测试||
|\*\#\*\#232338\#\*\#\*|显示WIFI网卡的MAC地址||
|\*\#\*\#1472365\#\*\#\*|快速GPS测试||
|\*\#\*\#1575\#\*\#\*|不同类型的GPS测试||
|\*\#\*\#232331\#\*\#\*|蓝牙测试||
|\*\#\*\#232337\#\*\#\*|显示蓝牙设备地址||

#### 系统固件版本信息

|代码|功能描述|备注|
|:-------:|:--------:|:-------:|
|\*\#\*\#1111\#\*\#\*|FTA软件版本||
|\*\#\*\#2222\#\*\#\*|FTA硬件版本||
|\*\#\*\#4986\*2650468\#\*\#\*|硬件信息||
|\*\#\*\#1234\#\*\#\*|PDA和手机系统信息||
|\*\#2263\#|基带选择||
|\*\#9090\#|诊断配置||
|*#7284#|USB模式控制||
|\*\#872564\#|USB日志控制||
|\*\#745\#|RIL日志输出菜单||
|\*\#746\#|调试日志输出菜单||
|\*\#9900\#|系统日志输出模式||
|\*\#\*\#44336\#\*\#\*|显示构建时间，更新列表||
|\*\#03\#|NAND闪存串号||
|\*\#3214789\#|GCF模式状态||
|\*\#4238378\#|GCF配置||


#### 工厂测试

|代码|功能描述|备注|
|:-------:|:--------:|:-------:|
|\*\#\*\#0283\#\*\#\*|网络数据包回路测试||
|\*\#\*\#0\*\#\*\#\*|LCD测试||
|\*\#\*\#0673\#\*\#\*|音频测试||
|\*\#\*\#0289\#\*\#\*|音频测试||
|\*\#\*\#0842\#\*\#\*|震动与背光测试||
|\*\#\*\#2663\#\*\#\*|触摸屏测试||
|\*\#\*\#2664\#\*\#\*|触摸屏测试||
|\*\#\*\#0588\#\*\#\*|近距离感应器测试||
|\*\#\*\#3264\#\*\#\*|内存硬件版本||
|\*\#0782\#|实时钟测试||
|\*\#0589\#|光感应器测试||
|\*\#7353\#|快速测试菜单||
|\*\#3214789650\#|LBS测试|
|\*\#8999\*8378\#|测试菜单|
|\*\#07\#|测试历史|

#### PDA和电话

|代码|功能描述|备注|
|:-------:|:--------:|:-------:|
|\*\#\*\#7262626\#\*\#\*|场测试||
|\*\#06\#|显示手机IMEI号码||
|\*\#\*\#8351\#\*\#\*|打开语音拨号记录日志||
|\*\#\*\#8350\#\*\#\*|关闭语音拨号记录日志||
|\*\*05\*\*\*\#|从紧急拨号屏幕解锁PUK码||
|\*\#301279\#|网络制式HSDPA HSUPA控制菜单||
|\*\#7465625\#|查看手机锁定状态||
|\*7465625\*638\*\#|配置网络锁定MCC/MNC|
|\*7465625\*782\*\#|配置网络锁定NSP|
|\*7465625\*77\*\#|插入网络锁定键SP|
|\*7465625\*27\*\#|插入网络锁定键NSP/CP|
|\*\#272886\#|自动接听选择|

#### 其他

|代码|功能描述|备注|
|:-------:|:--------:|:-------:|
|\*\#0\*\#|Galaxy S3服务菜单|Samsung|
|\*\#1234\#|软件版本|Samsung|
|\*\#12580\*369\#|硬件与软件信息|Samsung|
|\*\#0228\#|查看电池状态|Samsung|
|\*\#0011\#|打开服务菜单|Samsung|
|\*\#0283\#|网络回路测试|Samsung|
|\*\#0808\#|访问USB服务|Samsung|
|\*\#9090\#|打开服务模式|Samsung|
|\*\#7284\#|FactoryKeystring菜单|Samsung|
|\*\#34971539\#|访问摄像头系统|Samsung|
|\*\#7412365\#|摄像头固件菜单|Samsung|
|\#\#7764726|Motorola DROID 隐藏服务菜单|Motorola 默认密码6个0|
|1809\#\*990\#|LG Optimus 2x 隐藏服务菜单|LG 默认密码6个0|
|3845\#\*920\#|LG Optimus 3D 隐藏服务菜单|LG 默认密码6个0|
|3845\#\*850\#|LG G3 诊断测试菜单|LG AT&T|
|5689\#\*990\#|LG G3 诊断测试菜单|LG Sprint|
|3845\#\*851\#|LG G3 诊断测试菜单|LG T-Mobile|
|\#\#228378|LG G3 诊断测试菜单|LG Verizon|
|3845\#\*855\#|LG G3 诊断测试菜单|LG 网络变种|
|\*\#\*\#3424\#\*\#\*|HTC 测试功能|HTC|
|\#\#8626337\#|运行VOCODER|HTC|
|\#\#33284\#|场测试|HTC|
|\#\#3282\#|显示EPST菜单|HTC|
|\#\#3424\#|运行诊断模式|HTC|
|\#\#786\#|反转诊断支持|HTC|
|\#\#7738\#|协议修订|HTC|
|\*\#\*\#786\#\*\#\*|硬件重置|Nexus 5|
|\*\#\*\#7873778\#\*\#\*|启动Superuser应用|Nexus 5|
|\*\#\*\#1234\#\*\#\*|启动Superuser应用|Nexus 5|
|\*\#123\#|是否连接到家庭网络，仅用于加拿大和美国|Nexus 5|
|\*\#\*\#2432546\#\*\#\*|检查系统升级|Nexus 5|

## 如何使用蓝魔法
打开电话拨号盘，输入咒语，然后，芝麻开门。。。

## 修炼自己的蓝魔法
没错，你还可以自创咒语

在自己编写的APP清单文件(**AndroidManifest.xml**)中加入如下内容

```
<receiver android:name=".MySecretCodeReceiver">
    <intent-filter>
        <action android:name="android.provider.Telephony.SECRET_CODE" />
        <data android:scheme="android_secret_code" android:host="123456789" />
    </intent-filter>
</receiver>
```

**android:host**后面内容改成自己的咒语数字就可以啦，然后编译安装运行APP，打开拨号盘，输入咒语试试看呢。

## 一本比较全面的魔法书

[![Secret Codes on Google Play Store](https://play.google.com/intl/en_us/badges/images/generic/en-play-badge.png)](https://play.google.com/store/apps/details?id=com.ovmobile.secretcode) 

## 参考资料

[If you are an Android User, Than you Should try these 32 Secret Codes!](http://www.androidupdater.com/2015/02/if-you-are-android-user-than-you-should.html)  
[Android secret codes 2016 (Hidden tricks for hacking smartphone)](http://kimjohtechtricks.blogspot.com/2016/05/hidden-android-secret-codes-smartphone-hack.html)  
[Hidden Android Secret Codes For Samsung, HTC, Motorola, Sony, LG And Other Devices](http://www.redmondpie.com/hidden-android-secret-codes-for-samsung-htc-motorola-sony-lg-and-other-devices/)  
[Android Secret Codes 2016 | All Hidden Codes List](http://www.nextleveltricks.com/android-secret-codes/)  
[List of Hidden Android Secret Codes](http://www.thedroidtweaks.com/tip-n-tricks/list-of-hidden-android-secret-codes)  
[Android Secret Codes 2016 (Hidden Codes List)](http://www.safetricks.com/android-secret-codes/)  
[20 Android Secret Codes List 2015 For All Phones](http://www.smartphonetool.com/2015/06/20-android-secret-codes-list-2015.html)  
[Android Secret codes](http://www.nerdynaut.com/tech-mobile/android-secret-codes1)  
[Android Secret Codes](http://hardmasterreset.com/android-secret-codes/)  
[Android-SecretCodes](https://github.com/SimonMarquis/Android-SecretCodes)  
[Unstructured Supplementary Service Data(USSD)](https://en.wikipedia.org/wiki/Unstructured_Supplementary_Service_Data)  
[What’s the difference between USSD, MMI and SS codes?](https://berlin.ccc.de/~tobias/mmi-ussd-ss-codes-explained.html)  
[MMI Codes from android](http://www.cnblogs.com/mengshu-lbq/archive/2010/12/14/1905971.html)  
