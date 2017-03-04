---
layout:     post
title:      "JAVA decompiler collections"
subtitle:   "JAVA decompiler collections"  
date:       2015-11-24
author:     "figo"
header-img: "img/header/20151124.jpg"
header-mask: 0.5
catalog: true
tags:
    - Java
    - decompiler
    - jad
    - jadx
    - 2015
---

# Procyon  
open-source, [see more][1]  
Author: Mike Strobel  
Updated in 2015. Handles language enhancements from Java 5 and beyond, up to Java 8, including: 
Enum declarations
Enum and String switch statements
Local classes (both anonymous and named)
Annotations
Java 8 Lambdas and method references (i.e., the :: operator).
Java 7 is required to run.  

[1]: https://bitbucket.org/mstrobel/procyon/wiki/Java%20Decompiler

# CFR  
Free, no source-code available, [see more][2]  
Author: Lee Benfield  
Updated in 2015. CFR is able to decompile modern Java features - Java 8 lambdas (pre and post Java beta 103 changes), Java 7 String switches etc, but is written entirely in Java 6. 

[2]: http://www.benf.org/other/cfr/

# JD  
Free for non-commercial use only, [see more][3]  
Author: Emmanuel Dupuy  
Updated in 2014. Has its own visual interface and plugins to Eclipse and IntelliJ . Written in C++, so very fast. Supports Java 5.  

[3]: http://jd.benow.ca/
[4]: https://github.com/java-decompiler

# Fernflower  
open-source, [see more][5]  
Author: Egor Ushakov  
Updated in 2015. Very promising analytical Java decompiler, now becomes an integral part of IntelliJ 14. ([see more][6])  
Supports Java up to version 6 (Annotations, generics, enums)  

[5]: https://github.com/fesh0r/fernflower
[6]: https://github.com/JetBrains/intellij-community/tree/master/plugins/java-decompiler

# Krakatau  
open-source, [see more][7]  
Author: Robert Grosse  
written in Python, includes a robust verifier. It focuses on translating arbitrary byte code into valid Java code, as opposed to reconstructing the original code.

[7]: https://github.com/Storyyeller/Krakatau

# Candle  
open-source, [see more][8]  
Author: Brad Davis  
developer of JBoss Cake, is an early but promising work in progress.  

[8]: https://github.com/bradsdavis/candle-decompiler

# JAD  
given here only for historical reason. Free, no source-code available, [jad download mirror][9]  
Author: Pavel Kouznetsov  
Probably, this is the most popular Java decompiler, but primarily of this age only. Written in C++, so very fast. 
Outdated, unsupported and does not decompile correctly Java 5 and later.  

[9]: http://www.javadecompilers.com/jad

# Jadx  
Android application package (APK) is the package file format used to distribute and install application software onto Google's Android operating system.  
This site uses perfect open-source APK and DEX decompiler called Jadx, [see more][10]  
Jadx decompiles .class and .jar files, but also it produces Java source code from Android Dex and Apk files.  

[10]: https://sourceforge.net/projects/jadx/files/

# ClassyShark
open-source, [see more][11]  
Author: Boris Farber  
[ClassyShark](http://classyshark.com/) is a standalone tool for Android developers. It can reliably browse any Android executable and show important info such as class interfaces and members, dex counts and dependencies.  
The browser supports multiple formats including libraries (.dex, .aar, .so), executables (.apk, .jar, .class) and AndroidManifest (.xml).

[11]: https://github.com/google/android-classyshark
[12]: http://www.api-solutions.com/p/classyshark_6.html

# Bytecode Viewer
A Java 8 Jar & Android APK Reverse Engineering Suite (Decompiler, Editor, Debugger & More) [see more](https://github.com/Konloch/bytecode-viewer)  

# Enjarify
Enjarify is a tool for translating Dalvik bytecode to equivalent Java bytecode. This allows Java analysis tools to analyze Android applications. [see more](https://github.com/Storyyeller/enjarify)  

**Please, use it only for legitimate purposes.**

# References

[Decompilers online](http://www.javadecompilers.com)
