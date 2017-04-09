---
layout: post
title:  "GRADLE插件开发日记"
subtitle:   ""
date:       2017-04-09
author:     "figotan"
header-img: "img/header/20170409.jpg"
header-mask: 0.5
catalog: true
tags:
    - Gradle
    - Groovy
    - 2017
---

在Android Studio里开发Android APP，用Gradle来做项目的编译构建。有时候会在编译代码的过程中插入一些额外的流程（工具／服务）来完成定制的工作，把这部分流程独立出来，编写成一个插件，这样，可以在项目之间共享通用的程序。下面笔者介绍下如何快速的**Get**到这门技能。

## 前期准备  
#### Groovy语言的学习
Gradle插件由[Groovy](http://www.groovy-lang.org)语言编写而成，首先不妨花上一点时间大概了解下Groovy这门语言。推荐首先看看这篇文章[IBM developerWorks的精通Groovy](http://www.ibm.com/developerworks/cn/education/java/j-groovy/j-groovy.html)，大致过一遍有些了解就好，不用精读，文中的背景介绍，操作实例等都可以略过，重点看下关于无类型，循环范围，集合，映射，闭包等概念，有个初步的认识。这里啰嗦两句关于闭包的神奇之处。  
我们先看一段类似于**Java**的代码：  

``` Groovy
def acoll = ["Groovy", "Java", "Ruby"]
		
for(Iterator iter = acoll.iterator(); iter.hasNext();){
 println iter.next()
}
```

再看看更简洁更**正确**的姿势：  

``` Groovy
def acoll = ["Groovy", "Java", "Ruby"]
		
acoll.each{
 println it
}
```

由 **{}** 包围起来的代码块就是**闭包**，这样写代码是不是感觉工作效率很高？  
闭包中的**it**变量是一个关键字（Groovy语言内置的默认值），指向被调用的外部集合的每个值，可以用传递给闭包的参数覆盖它（一般不用）。  

下面的代码执行同样的操作，但使用自己定义的变量：  

``` Groovy
def acoll = ["Groovy", "Java", "Ruby"]
		
acoll.each{ value ->
 println value
}
```

在这个示例中，用**value**代替了Groovy的默认**it**。  

总结下Groovy的特点  

* 代码结束无需分号
* 变量以及函数／方法返回值无需声明类型
* 除非指定，所有成员均为public
* 一个脚本中可以不用定义class
* 闭包

接下来进阶一点的学习,如果你有**Java**的编程基础，那么看看[这篇Groovy和Java的不同之处](http://www.groovy-lang.org/differences.html)，上手会快很多。建议精读，因为很多将会是你以后编程的时候遇到的迷惑和坑。

还有一些文章是可以作为炕头书的，在编码的过程中，如果你卡壳了，遇到了一些不知道该如何做的事情，可以立即打开查阅一下

* [Groovy开发工具集](http://www.groovy-lang.org/groovy-dev-kit.html)
* [Groovy设计模式](http://www.groovy-lang.org/design-patterns.html)

#### 了解Android APP的构建流程
关于这部分详细内容网上已经有很多文章介绍了，这里不再赘述，可以查看本文**参考文章**里的引用  
这里罗列一些在编程的过程中可能需要参考的案头资料，以备不时之需：  

* [Gradle Plugin User Guide](http://tools.android.com/tech-docs/new-build-system/user-guide)
* [Android Plugin DSL Reference](http://google.github.io/android-gradle-dsl/current/)

贴一张官方提供的编译构建流程图，大家可以大致观摩了解下
![构建流程图](http://images.figotan.org/image.php?di=RY7D)

## 编写一个安卓Gradle插件
简述下大概的流程  

* 在Android Studio里新建一个项目，然后创建一个Android Library的模块（Android APP，Java Library也可以，创建一个module就好），项目名字自己定义（这里的demo名字叫做mygradleplugin）
* 将这个module的build.gradle的内容修改成为：

```
apply plugin: 'groovy'
apply plugin: 'maven'

dependencies {
    compile gradleApi()
    compile localGroovy()
}

repositories {
    mavenCentral()
}

group='com.your.name'
version='1.0.0'

uploadArchives {
    repositories {
        mavenDeployer {
            repository(url: uri('../gradle_plugins'))
        }
    }
}
```

**repository(url: uri('../gradle_plugins'))**这句话表示生成的插件内容放在主工程的gradle_plugins目录下，方便本地调试，等到插件编写调试完成后，可以通过maven插件发布到公司内部或者外面的公共repo仓库中，注意**group**和**version**是maven里表示一个artifact的GVA坐标，也是引用插件需要的  

* 将java目录改名为groovy目录，在src/main/groovy/com/your/name/mygradleplugin下新建如下三个groovy文件，分别表示插件入口，外部传递给插件的参数和插件提供的任务  
插件执行的入口类，内容如下  

```
package com.your.name.mygradleplugin

import org.gradle.api.Plugin
import org.gradle.api.Project

public class MyGradlePlugin implements Plugin<Project> {
    void apply(Project project) {
        project.extensions.create('MyGradlePluginParams', MyGradlePluginExtension)
        project.task('myGradlePluginTask', type:MyGradlePluginTask)
    }
}
```

外部传递给插件的参数类，内容如下  

```
package com.your.name.mygradleplugin

class MyGradlePluginExtension {
    def param = "param defaut"
}
```

插件提供的任务，内容如下  

```
package com.your.name.mygradleplugin

import org.gradle.api.DefaultTask
import org.gradle.api.tasks.TaskAction

public class MyGradlePluginTask extends DefaultTask {
    MyGradlePluginExtension configuration

    ImagesOptTask() {
        configuration = project.MyGradlePluginParams
    }

    @TaskAction
    void run() {
        println configuration.param
    }
}
```

* 在插件module的src/main/目录下创建目录resources/META-INF/gradle-plugins/，在这个目录下新建文件com.your.name.mygradleplugin.properties，写入如下内容

```
implementation-class=com.your.name.mygradleplugin.MyGradlePlugin
```

这句话是为了告诉Gradle，插件的入口类是哪个

一个简单的插件编写完成，功能是提供给Gradle一个叫做myGradlePluginTask的任务，该任务接收传递给它的参数，然后将这个参数内容打印出来

* 执行插件module的**uploadArchives**任务，会在项目更目录的**gradle_plugins**下生成插件的内容
* 在主项目中引用插件并测试，在工程根目录的**build.gradle**文件中加入如下内容

```
buildscript {
    repositories {
        maven {
            url uri('../gradle_plugins')
        }
    }
    dependencies {
        classpath 'com.your.name:mygradleplugin:1.0.0'
    }
}
apply plugin: 'com.your.name.mygradleplugin'

MyGradlePluginParams {
    param = 'Hello world, my first gradle plugin!'
}
```

* **url uri('../gradle_plugins')**表示引用插件的地址  
* **classpath 'com.your.name:mygradleplugin:1.0.0'**表示插件artifact的GVA坐标，分别由前面定义的**group**和**version**以及模块名称决定  
* **apply plugin: 'com.your.name.mygradleplugin'**由插件resources/META-INF/gradle-plugins/目录下文件com.your.name.mygradleplugin.properties的名字决定  
* MyGradlePluginParams是传递给插件的参数以及内容

* 执行 ./gradlew myGradlePluginTask命令，输出

```
Hello world, my first gradle plugin!
```

一个简单的插件就这样编写完成

## 参考文章
[如何使用Android Studio开发Gradle插件](http://blog.csdn.net/sbsujjbcy/article/details/50782830)  
[Android自定义Gradle插件](http://smallsoho.com/2017/03/18/Android自定义Gradle插件.html)  
[自定义Gradle插件（一）](http://blog.csdn.net/liuhongwei123888/article/details/50541759)  
[自定义Gradle插件（二）](http://blog.csdn.net/liuhongwei123888/article/details/50542104)  
[构建神器Gradle](http://jiajixin.cn/2015/08/07/gradle-android/)  

