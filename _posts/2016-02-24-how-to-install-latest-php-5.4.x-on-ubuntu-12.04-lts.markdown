---
layout: post
title:  "How to install latest PHP 5.4.x on Ubuntu 12.04 LTS"
subtitle:   ""  
date:       2016-02-24
author:     "figo"
header-img: ""
tags:
    - vps
    - ubuntu
    - php
    - 2016
---
1. Add this package-repository to your system. If Ubuntu says that you need to download a key first, then follow the instructions given in the notice.
{% highlight bash %}
sudo add-apt-repository ppa:ondrej/php5-oldstable
{% endhighlight %}
If you get an error message now, then please do an update first and install the python-software-properties, that need to be necessary to add a package repository:
{% highlight bash %}
sudo apt-get update
sudo apt-get install python-software-properties
{% endhighlight %}
2. Update
{% highlight bash %}
sudo apt-get update
{% endhighlight %}
Check the available version of PHP (the result is self-explaining, the version on the top is the one that will be installed):
{% highlight bash %}
apt-cache policy php5
{% endhighlight %}
3. Install PHP 5.4.x
{% highlight bash %}
sudo apt-get install php5
{% endhighlight %}
Check the installed version of PHP (if this does not show 5.4.x please restart your apache)
{% highlight bash %}
php5 -v
{% endhighlight %}
Please note: The ondrej/php5-oldstable repository (which is used here) provides the very latest version of the PHP 5.4-branch. Usually a version-update is available a few days after it was been officially released. This is really cool and a big step forward as Ubuntu, Debian, CentOS etc. provide only very old versions by default.
