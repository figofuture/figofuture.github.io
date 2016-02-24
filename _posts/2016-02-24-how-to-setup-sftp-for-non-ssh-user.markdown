---
layout: post
title:  "在Linux中为非SSH用户配置SFTP环境"
subtitle:   ""  
date:       2016-02-24
author:     "figo"
header-img: ""
tags:
    - vps
    - linux
    - ssh
    - sftp
    - chroot
    - 2016
---
在某些环境中，系统管理员想要允许极少数用户在可以传输文件到Linux机器中，但是不允许使用 SSH。要实现这一目的，我们可以使用SFTP，并为其构建chroot环境。

SFTP & chroot背景：

SFTP是指SSH文件传输协议（SSH File Transfer protocol）或安全文件传输协议（Secure File Transfer Protocol），它提供了可信数据流下的文件访问、文件传输以及文件管理功能。当我们为SFTP配置chroot环境后，只有被许可的用户可以访问，并被限制到他们的主目录中，换言之：被许可的用户将处于牢笼环境中，在此环境中它们甚至不能切换它们的目录。

在本文中，我们将配置Ubuntu 12.04.5 LTS中的SFTP Chroot环境。我们开启一个用户帐号‘Guest’，该用户将被允许在Linux机器上传输文件，但没有ssh访问权限。

步骤：1 创建组
{% highlight bash %}
$ sudo groupadd  sftp_users
{% endhighlight %}
步骤：2 分配附属组(sftp_users)给用户

如果用户在系统上不存在，使用以下命令创建（ 这里给用户指定了一个不能登录的 shell，以防止通过 ssh 登录）：
{% highlight bash %}
$ sudo useradd  -G sftp_users  -s /bin/false  guest
$ sudo passwd guest
{% endhighlight %}
对于已经存在的用户，使用以下usermod命令进行修改：
{% highlight bash %}
$ sudo usermod –G sftp_users  -s /bin/false  guest
{% endhighlight %}
注意：如果你想要修改用户的默认主目录，那么可以在useradd和usermod命令中使用‘-d’选项，并设置合适的权限。

步骤：3 现在编辑配置文件 “/etc/ssh/sshd_config”
{% highlight bash %}
$ sudo vi /etc/ssh/sshd_config
{% endhighlight %}
{% highlight bash %}
 #comment out the below line and add a line like below
 #Subsystem sftp /usr/libexec/openssh/sftp-server  
 Subsystem sftp internal-sftp
 
 # add Below lines  at the end of file 
 Match Group sftp_users  
 X11Forwarding no  
 AllowTcpForwarding no 
 ChrootDirectory %h     
 ForceCommand internal-sftp 
{% endhighlight %}
此处：

Match Group sftp_users – 该参数指定以下的行将仅仅匹配sftp_users组中的用户
ChrootDirectory %h – 该参数指定用户验证后用于chroot环境的路径（默认的用户主目录）。对于用户 Guest，该路径就是/home/guest。
ForceCommand internal-sftp – 该参数强制执行内部sftp，并忽略任何~/.ssh/rc文件中的命令。
重启ssh服务
{% highlight bash %}
$ sudo service ssh restart
{% endhighlight %}
步骤：4 设置权限：
{% highlight bash %}
$ sudo chmod 755 /home/guest
$ sudo chown root /home/guest
$ sudo chgrp -R sftp_users /home/guest
{% endhighlight %}
如果你想要允许guest用户上传文件，那么创建一个上传文件夹，设置权限如下：
{% highlight bash %}
$ sudo mkdir /home/guest/upload
$ sudo chown guest. /home/guest/upload/
{% endhighlight %}
步骤：5 现在尝试访问系统并进行测试

via: http://www.linuxtechi.com/configure-chroot-sftp-in-linux/

https://linux.cn/article-3692-1.html
