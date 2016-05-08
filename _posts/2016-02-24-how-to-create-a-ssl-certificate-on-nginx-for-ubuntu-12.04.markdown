---
layout: post
title:  "How To Create a SSL Certificate on nginx for Ubuntu 12.04"
subtitle:   ""  
date:       2016-02-24
author:     "figo"
header-img: ""
tags:
    - vps
    - ubuntu
    - https
    - ssl
    - nginx
    - 2016
---
About Self-Signed Certificates
A SSL certificate is a way to encrypt a site’s information and create a more secure connection. Additionally, the certificate can show the virtual private erver’s identification information to site visitors. Certificate Authorities can issue SSL certificates that verify the server’s details while a self-signed certificate has no 3rd party corroboration.

Set Up
The steps in this tutorial require the user to have root privileges. You can see how to set that up in the Initial Server Setup Tutorial in steps 3 and 4.

Additionally, you need to have nginx already installed and running on your VPS. If this is not the case, you can download it with this command:

sudo apt-get install nginx
Step One—Create a Directory for the Certificate
The SSL certificate has 2 parts main parts: the certificate itself and the public key. To make all of the relevant files easy to access, we should create a directory to store them in:

sudo mkdir /etc/nginx/ssl
We will perform the next few steps within the directory:

cd /etc/nginx/ssl
Step Two—Create the Server Key and Certificate Signing Request
Start by creating the private server key. During this process, you will be asked to enter a specific passphrase. Be sure to note this phrase carefully, if you forget it or lose it, you will not be able to access the certificate.

sudo openssl genrsa -des3 -out server.key 1024
Follow up by creating a certificate signing request:

sudo openssl req -new -key server.key -out server.csr
This command will prompt terminal to display a lists of fields that need to be filled in.

The most important line is “Common Name”. Enter your official domain name here or, if you don’t have one yet, your site’s IP address. Leave the challenge password and optional company name blank.

You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter ‘.’, the field will be left blank.
—–
Country Name (2 letter code) [AU]:US
State or Province Name (full name) [Some-State]:New York
Locality Name (eg, city) []:NYC
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Awesome Inc
Organizational Unit Name (eg, section) []:Dept of Merriment
Common Name (e.g. server FQDN or YOUR name) []:example.com
Email Address []:webmaster@awesomeinc.com
Step Three—Remove the Passphrase
We are almost finished creating the certificate. However, it would serve us to remove the passphrase. Although having the passphrase in place does provide heightened security, the issue starts when one tries to reload nginx. In the event that nginx crashes or needs to reboot, you will always have to re-enter your passphrase to get your entire web server back online.

Use this command to remove the password:

sudo cp server.key server.key.org
sudo openssl rsa -in server.key.org -out server.key
Step Four— Sign your SSL Certificate
Your certificate is all but done, and you just have to sign it. Keep in mind that you can specify how long the certificate should remain valid by changing the 365 to the number of days you prefer. As it stands this certificate will expire after one year.

sudo openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
You are now done making your certificate.

Step Five—Set Up the Certificate
Now we have all of the required components of the finished certificate.The next thing to do is to set up the virtual hosts to display the new certificate.

Let’s create new file with the same default text and layout as the standard virtual host file. You can replace “example” in the command with whatever name you prefer:

sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/example
Then go ahead and open up that new file:

sudo nano /etc/nginx/sites-available/example
Scroll down to the bottom of the file and find the section that begins with this:

# HTTPS server

server {
listen 443;
server_name example.com;

root /usr/share/nginx/www;
index index.html index.htm;

ssl on;
ssl_certificate /etc/nginx/ssl/server.crt;
ssl_certificate_key /etc/nginx/ssl/server.key;
}
Uncomment within the section under the line HTTPS Server. Match your config to the information above, replacing the example.com in the “server_name” line with your domain name or IP address. Subsequently, add in the correct directory for your site (the above configuration includes the default nginx page).

Additionally, make sure that both of these lines are commented out in the line toward the beginning of the file that says:

# Make site accessible from http://localhost/
# server_name localhost;
Step Six—Activate the Virtual Host
The last step is to activate the host by creating a symbolic link between the sites-available directory and the sites-enabled directory.

sudo ln -s /etc/nginx/sites-available/example /etc/nginx/sites-enabled/example
Then restart nginx:

sudo service nginx restart
Visit https://youraddress

You will see your self-signed certificate on that page!

See More
Once you have setup your SSL certificate on the site, you can Install an FTP server if you haven’t done so yet.

Resources
http://wiki.nginx.org/HttpSslModule#Generate_Certificates
