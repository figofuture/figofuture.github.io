---
layout: post
title:  "How To Install phpMyAdmin on a LEMP server"
subtitle:   ""  
date:       2016-02-24
author:     "figo"
header-img: ""
tags:
    - vps
    - ubuntu
    - lemp
    - lnmp
    - php
    - phpMyAdmin
    - 2016
---
About phpMyAdmin
phpMyAdmin is a free software to work with MySQL on the web—it provides a convenient visual front end to the capabilities of MySQL.

Setup
Prior to installing phpMyAdmin, be sure that you have LEMP installed on your VPS. If you do not, you can see how to do that here.

Once you have finished installing LEMP on your virtual private server, you can start on installing phpMyAdmin.

Step One—Install phpMyAdmin
Start off by downloading the program from apt-get.

sudo apt-get install phpmyadmin
During the installation, phpmyadmin will ask you if you want to configure the database with dbconfig. Go ahead and choose yes.

Input MySQL’s database password when prompted and click ok.

When phpmyadmin prompts you to choose a server (either apache or lighttpd) hit tab, and select neither one.

Step Two—Configure phpMyAdmin
You now have phpMyAdmin installed on your server. In order to access it, you need to take one more step.

Create a symbolic link between phpMyAdmin and your site’s directory. If you were using the previous tutorial, it may be still located in the nginx default directory, otherwise link it with the appropriate place:

sudo ln -s /usr/share/phpmyadmin/ /usr/share/nginx/www
Restart nginx:

sudo service nginx restart
You should now be able to access phpMyAdmin by going to yourdomain/phpmyadmin. The screen will look like this.

Log in with your MySQL username and password.
