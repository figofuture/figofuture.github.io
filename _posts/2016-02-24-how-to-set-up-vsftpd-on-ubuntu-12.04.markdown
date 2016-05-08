---
layout: post
title:  "How To Set Up vsftpd on Ubuntu 12.04"
subtitle:   ""  
date:       2016-02-24
author:     "figo"
header-img: ""
tags:
    - vps
    - ubuntu
    - vsftpd
    - ftp
    - 2016
---
About vsftpd
Warning: FTP is inherently insecure. If you must use FTP, consider securing your FTP connection with SSL/TLS. Otherwise, it is best to use SFTP, a secure alternative to FTP.

The first two letters of vsftpd stand for “very secure” and the program was built to have strongest protection against possible FTP vulnerabilities.

Step One—Install vsftpd
You can quickly install vsftpd on your virtual private server in the command line:

sudo apt-get install vsftpd
Once the file finishes downloading, the VSFTP will be on your droplet. Generally speaking, it is already configured with a reasonable amount of security. However, it does provide access on your VPS to anonymous users.

Step Two—Configure vsftpd
Once vsftpd is installed, you can adjust the configuration.

Open up the configuration file:

sudo nano /etc/vsftpd.conf
The biggest change you need to make is to switch the Anonymous_enable from YES to NO:

anonymous_enable=NO
Prior to this change, vsftpd allowed anonymous, unidentified users to access the server’s files. This is useful if you are seeking to distribute information widely, but may be considered a serious security issue in most other cases.

After that, uncomment the local_enable option, changing it to yes and, additionally, allow the user to write to the directory.

local_enable=YES
write_enable=YES
Finish up by uncommenting command to chroot_local_user. When this line is set to Yes, all the local users will be jailed within their chroot and will be denied access to any other part of the server.

chroot_local_user=YES
Save and Exit that file.

Because of a recent vsftpd upgrade, vsftpd is “refusing to run with writable root inside chroot”. A handy way to address this issue to is to take the following steps:

Create a new directory within the user’s home directory
mkdir /home/username/files
Change the ownership of that file to root
chown root:root /home/username
Make all necessary changes within the “files” subdirectory
Then, as always, restart:

sudo service vsftpd restart
Step Three—Access the FTP server
Once you have installed the FTP server and configured it to your liking, you can now access it.

You can reach an FTP server in the browser by typing the domain name into the address bar and logging in with the appropriate ID. Keep in mind, you will only be able to access the user’s home directory.

ftp://example.com
Alternatively, you can reach the FTP server on your virtual server through the command line by typing:

ftp example.com
Then you can use the word, “exit,” to get out of the FTP shell.
