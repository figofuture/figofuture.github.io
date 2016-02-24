---
layout: post
title:  "How To Install authpuppy on Nginx"
subtitle:   ""  
date:       2016-02-24
author:     "figo"
header-img: ""
tags:
    - linux
    - gentoo
    - authpuppy
    - nginx
    - 2016
---
As we know the authpuppy official [wiki website][1] only supply the installation guide on apache web server, but as a good result as I practiced, it can be successfully installed on the nginx web server.

Go on details, my linux server using Gentoo Linux, but i think it works on any linux popular distribution, Redhat, centos, debian, ubuntu, arch etc.

Step by step go on,

First, setup your NGINX web server, and I show you my nginx configuration
{% highlight nginx %}
server {

listen   [your server ip]:80;

index index.php index.html index.htm;

server_name www.[your server name] [your server name] *.[your server name];

access_log /var/log/nginx/[your server name].access_log main;

error_log /var/log/nginx/[your server name].error_log info;

set $domain [your server name];

set $subdomain "";

if ( $host ~* (\b(?!www\b)\w+)\.\w+\.[a-zA-Z]+$ ) {

set $subdomain /$1;

}

client_max_body_size 200M;

root /var/www/$domain$subdomain;

location / {

#if ($domain !~* ^www$) {

#rewrite ^/(.*)    /$domain/$1 break;

#}

try_files $uri $uri/ /index.php?q=$uri&$args;

}

error_page 404 /404.html;

error_page 500 502 503 504 /50x.html;

 

location = /50x.html {

root /usr/share/nginx/www;

}

# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000

location ~ \.php$ {

# With php5-fpm:

fastcgi_pass unix:/var/run/php5-fpm.sock;

fastcgi_index index.php;

fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;

#fastcgi_param SCRIPT_FILENAME /var/www/$domain$subdomain$fastcgi_script_name;

include fastcgi_params;

}

}
{% endhighlight %}
check nginx is OK.

Next, download the authpuppy source codes from authpuppy official website
{% highlight bash %}
# wget -c [authpuppy-1.0.0-stable.tgz][2]

# tar xf authpuppy-1.0.0-stable.tgz

# chown -R www. authpuppy

# cd authpuppy

# ln -s /[your home]/authpuppy/lib/vendor/symfony/data/web/sf sf

# chown -h www. sf
{% endhighlight %}
Next, goto your web server virtual hosts folder
{% highlight bash %}
# ln -s /[your home]/authpuppy/web authpuppy

# chown -h www. authpuppy
{% endhighlight %}
Nest setup database
{% highlight bash %}
# mysqladmin -uroot -p create authpuppy

# mysql -uroot -p
{% endhighlight %}
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 45
Server version: 5.0.51a-3ubuntu5.5 (Ubuntu)
   
Type 'help;' or '\h' for help. Type '\c' to clear the buffer.

``` mysql   
mysql> create user 'authpuppy'@'localhost' identified by 'authpuppydev';
```

Query OK, 0 rows affected (0.21 sec)

``` mysql   
mysql> grant all privileges on authpuppy.* to 'authpuppy'@'localhost' with grant option;
```

Query OK, 0 rows affected (0.02 sec)

You may now navigate to http://<your authpuppy server>/

If the database is not configured yet, the file will be created. A check of requirements will also automatically be made. Make sure all required requirements are met.

Follow the instructions on the screen through the installation process. If it is a first install, you’ll be invited to create a first admin user for the application

Congratulations! You’re now ready to go! Just click on the “Administrative Login” link to get the login prompt for the admin section and access more interesting stuff.

[1]: http://www.authpuppy.org/doc/Getting_Started
[2]: https://launchpad.net/authpuppy/trunk/1.0.0-stable/+download/authpuppy-1.0.0-stable.tgz
