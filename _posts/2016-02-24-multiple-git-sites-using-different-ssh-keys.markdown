---
layout: post
title:  "Multiple git sites using different ssh keys"
subtitle:   ""  
date:       2016-02-24
author:     "figo"
header-img: ""
tags:
    - ssl
    - git
    - 2016
---
First, you must use ssh-keygen to generate ssh key for your different accessing git sites. Like github, gitlab, or gitcafe etc.
{% highlight bash %}
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
{% endhighlight %}
Finally, edit ~/.ssh/config to add configuration to specific git site
{% highlight bash %}
$ vi ~/.ssh/config
{% endhighlight %}
{% highlight bash %}
Host gitlab.com
    RSAAuthentication yes
    IdentityFile ~/my-ssh-key-directory/my-gitlab-private-key-filename
    User git
Host git.assembla.com
    RSAAuthentication yes
    IdentityFile ~/.ssh/id_rsa_assembla
    User git
{% endhighlight %}
