---
layout: post
title:  "How To Build openwrt for WR841N on linux from source codes"
subtitle:   ""  
date:       2016-02-24 15:48:00
author:     "figo"
header-img: ""
tags:
    - linux
    - openwrt
    - wr841n
    - 2016
---
First, setup build environment, different linux distribution use different method.
ignore…
Next, fetch the openwrt surce code
{% highlight bash %}
# git clone git://git.openwrt.org/openwrt.git
{% endhighlight %}
Next, install extra packages
{% highlight bash %}
# ./scripts/feeds update -a
# ./scripts/feeds install -a
{% endhighlight %}
Next, config the source codes
{% highlight bash %}
# make menuconfig
{% endhighlight %}
{% highlight bash %}
Target System---AR71xx/AR7240/AR913x/AR934x
Target Profile---TP-LINK WR841N/ND
LuCI—>Collections—– <*> luci
{% endhighlight %}
Next, build
{% highlight bash %}
# make V=99
{% endhighlight %}
outputs are under ./bin
