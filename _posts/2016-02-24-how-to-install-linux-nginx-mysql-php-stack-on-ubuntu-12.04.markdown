---
layout: post
title:  "How To Install Linux, nginx, MySQL, PHP (LEMP) stack on Ubuntu 12.04"
subtitle:   ""  
date:       2016-02-24
author:     "figo"
header-img: ""
tags:
    - vps
    - ubuntu
    - lnmp
    - php
    - nginx
    - mysql
    - lemp
    - fpm
    - 2016
---
About Lemp
LEMP stack is a group of open source software to get web servers up and running. The acronym stands for Linux, nginx (pronounced Engine x), MySQL, and PHP. Since the server is already running Ubuntu, the linux part is taken care of. Here is how to install the rest.

Setup
The steps in this tutorial require the user to have root privileges. You can see how to set that up in the Initial Server Setup Tutorial in steps 3 and 4.

Step One—Update Apt-Get
Throughout this tutorial we will be using apt-get as an installer for all the server programs. On May 8th, 2012, a serious php vulnerability was discovered, and it is important that we download all of the latest patched software to protect the virtual private server.

Let’s do a thorough update.

sudo apt-get update
Step Two—Install MySQL
MySQL is a powerful database management system used for organizing and retrieving data

To install MySQL, open terminal and type in these commands:

sudo apt-get install mysql-server php5-mysql
During the installation, MySQL will ask you to set a root password. If you miss the chance to set the password while the program is installing, it is very easy to set the password later from within the MySQL shell.

Once you have installed MySQL, we should activate it with this command:

sudo mysql_install_db
Finish up by running the MySQL set up script:

sudo /usr/bin/mysql_secure_installation
The prompt will ask you for your current root password.

Type it in.

Enter current password for root (enter for none):
OK, successfully used password, moving on…
Then the prompt will ask you if you want to change the root password. Go ahead and choose N and move on to the next steps.

It’s easiest just to say Yes to all the options. At the end, MySQL will reload and implement the new changes.

By default, a MySQL installation has an anonymous user, allowing anyone
to log into MySQL without having to have a user account created for
them. This is intended only for testing, and to make the installation
go a bit smoother. You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
… Success!

Normally, root should only be allowed to connect from ‘localhost’. This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
… Success!

By default, MySQL comes with a database named ‘test’ that anyone can
access. This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
– Dropping test database…
… Success!
– Removing privileges on test database…
… Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
… Success!

Cleaning up…
Once you’re done with that you can finish up by installing PHP.

Step Three—Install nginx
Once MySQL is all set up, we can move on to installing nginx on the VPS.

echo “deb http://ppa.launchpad.net/nginx/stable/ubuntu $(lsb_release -sc) main” | sudo tee /etc/apt/sources.list.d/nginx-stable.list
sudo apt-key adv –keyserver keyserver.ubuntu.com –recv-keys C300EE8C
sudo apt-get update
sudo apt-get install nginx
nginx does not start on its own. To get nginx running, type:

sudo service nginx start
You can confirm that nginx has installed an your web server by directing your browser to your IP address.

You can run the following command to reveal your VPS’s IP address.

ifconfig eth0 | grep inet | awk ‘{ print $2 }’
Step Four—Install PHP
To install PHP-FPM, open terminal and type in these commands. We will configure the details of nginx and php details in the next step:

sudo apt-get install php5-fpm
Step Five—Configure php
We need to make one small change in the php configuration.Open up php.ini:
sudo nano /etc/php5/fpm/php.ini
Find the line, cgi.fix_pathinfo=1, and change the 1 to 0.
cgi.fix_pathinfo=0
If this number is kept as 1, the php interpreter will do its best to process the file that is as near to the requested file as possible. This is a possible security risk. If this number is set to 0, conversely, the interpreter will only process the exact file path—a much safer alternative. Save and Exit. We need to make another small change in the php5-fpm configuration.Open up www.conf:
sudo nano /etc/php5/fpm/pool.d/www.conf
Find the line, listen = 127.0.0.1:9000, and change the 127.0.0.1:9000 to /var/run/php5-fpm.sock.
listen = /var/run/php5-fpm.sock
Save and Exit.

Restart php-fpm:

sudo service php5-fpm restart
Step Six—Configure nginx
Open up the default virtual host file.

sudo nano /etc/nginx/sites-available/default
The configuration should include the changes below (the details of the changes are under the config information):

UPDATE: Newer Ubuntu versions create a directory called ‘html’ instead of ‘www’ by default. If /usr/share/nginx/www does not exist, it’s probably called html. Make sure you update your configuration appropriately.

[…] server {
listen 80;

root /usr/share/nginx/www;
index index.php index.html index.htm;

server_name example.com;

location / {
try_files $uri $uri/ /index.html;
}

error_page 404 /404.html;

error_page 500 502 503 504 /50x.html;
location = /50x.html {
root /usr/share/nginx/www;
}

# pass the PHP scripts to FastCGI server listening on the php-fpm socket
location ~ \.php$ {
try_files $uri =404;
fastcgi_pass unix:/var/run/php5-fpm.sock;
fastcgi_index index.php;
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
include fastcgi_params;

}

}
[…] Here are the details of the changes:

Add index.php to the index line.
Change the server_name from local host to your domain name or IP address (replace the example.com in the configuration)
Change the correct lines in “location ~ \.php$ {“ section
Save and Exit

Step Seven—Create a php Info Page
We can quickly see all of the details of the new php configuration.

To set this up, first create a new file:

sudo nano /usr/share/nginx/www/info.php
Add in the following line:

Then Save and Exit.

Restart nginx

sudo service nginx restart
You can see the nginx and php-fpm configuration details by visiting http://youripaddress/info.php

Your LEMP stack is now set up and configured on your virtual private server.

See More
After installing LEMP, you can Install WordPress, go on to do more with MySQL (A Basic MySQL Tutorial) or Install phpMyAdmin, Create an SSL Certificate, or Install an FTP Server.
