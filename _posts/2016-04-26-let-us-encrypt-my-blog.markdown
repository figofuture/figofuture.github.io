---
layout: post
title:  "使用Let’s Encrypt给网站穿上HTTPS的铠甲"
subtitle:   ""
date:       2016-04-26
author:     "figotan"
header-img: "img/header/20160426.png"
header-mask: 0.5
catalog: true
tags:
    - letsencrypt
    - gitlab
    - https
    - ssl
    - gentoo
    - nginx
    - 2016
---

# 首次生成证书
![](https://letsencrypt.org/images/letsencrypt-logo-horizontal.svg)  
从Github签出[Let’s Encrypt](https://github.com/letsencrypt/letsencrypt)的源代码

```bash
git clone https://github.com/letsencrypt/letsencrypt
```

进入本地源代码目录

```bash
cd letsencrypt
```

Let’s Encrypt提供多种认证方式，因为之前在VPS上有了HTTP的网站，所以这里采用了**webroot**的方式，其他方式请参考[官方文档](https://letsencrypt.readthedocs.org/en/latest/using.html#plugins)  
如果是主域名的认证：

```bash
./letsencrypt-auto --debug certonly --webroot --email name@your_main_domain.com -d www.your_main_domain.com -d your_main_domain.com -w /var/www/your_main_domain.com
```

子域名的认证：

```bash
./letsencrypt-auto --debug certonly --webroot --email name@your_main_domain.com -d subdomain.your_main_domain.com -w /var/www/your_main_domain.com/subdomain
```

然后在弹出的蓝底白字提示框中一路点击"OK"

注意如下问题：

* 请将命令中的**name**, **your_main_domain.com**, **subdomain**替换成你自己的名字，域名以及子域名  
* 因为Gentoo目前是在试验阶段，所以命令行加上**--debug**参数
* 参数**--email**如果没有在命令行加上，会在随后弹出的对话框里提示你填写
* **-w**指定Web服务器网址内容放置的目录，请指定自己放置的目录

生成的证书放在**/etc/letsencrypt/live/[网站域名]**下  

| 文件名     | 内容             | 
|:-------------:|:----------------------------------:|
| cert.pem      | 服务端证书       |
| chain.pem     | 浏览器需要的所有证书但不包括服务端证书，比如根证书和中间证书           |
| fullchain.pem | 包括了cert.pem和chain.pem的内容 |
| privkey.pem   | 证书的私钥|

一般情况下**fullchain.pem**和**privkey.pem**就够用了

# Web服务端配置
采用的[Nginx](http://nginx.org)做的Web服务器，所以这里贴下我的服务端配置

因为同一个VPS上放置了多个站点，所以Nginx采用vhost的方式，将主域名以及子域名的Nginx配置文件单独编写，并放置在**/etc/nginx/sites-available/**下，如果哪个网站启用，则在/etc/nginx/sites-enabled/下创建**/etc/nginx/sites-available/**对应目录的软链接。这一套原本是Ubuntu发行版里面Nginx的默认配置，个人觉得不错，所以照搬到Gentoo里使用。

Nginx的主配置文件**/etc/nginx/nginx.conf**需要加入如下配置：

``` nginx
http {
    
    ##
    # Virtual Host Configs
    ##

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;

}
```

子域名vhost的配置，在**/etc/nginx/sites-available/**下创建文件subdomian并填入如下内容

``` nginx
server {
    listen 443 ssl;
    server_name  subdomain.your_main_domain.com;
    # http://www.acunetix.com/blog/articles/configure-web-server-disclose-identity/
    server_tokens off;
    access_log /var/log/nginx/subdomain.your_main_domain.com.access_log;
    error_log /var/log/nginx/subdomain.your_main_domain.com.error_log;
    index index.html index.htm index.php;
    root /var/www/your_main_domain.com/subdomain;

    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;
    ssl_session_cache builtin:1000 shared:SSL:10m;
    resolver 8.8.8.8 8.8.4.4 valid=300s;
    resolver_timeout 5s;
    ssl_prefer_server_ciphers on;
    ssl_certificate /etc/letsencrypt/live/subdomain.your_main_domain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/subdomain.your_main_domain.com/privkey.pem;
    ssl_session_timeout 5m;

    ssl_session_tickets      on;
    ssl_stapling             on;
    ssl_stapling_verify      on;

    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload";
    add_header X-Frame-Options deny;
    add_header X-Content-Type-Options nosniff;
    add_header               Content-Security-Policy "default-src 'none'; script-src 'unsafe-inline' 'unsafe-eval' blob: https:; img-src data: https: http://ip.qgy18.com; style-src 'unsafe-inline' https:; child-src https:; connect-src 'self' https://translate.googleapis.com; frame-src https://disqus.com https://www.slideshare.net";

    # https://www.troyhunt.com/shhh-dont-let-your-response-headers/
    proxy_hide_header        Vary;
    fastcgi_hide_header      X-Powered-By;
    fastcgi_hide_header      X-Runtime;
    fastcgi_hide_header      X-Version;

    include php-fpm.conf;
}

server {
    server_name subdomain.your_main_domain.com;
    listen 80;
    server_tokens     off;
    access_log        /dev/null;
    location ^~ /.well-known/acme-challenge/ {
        alias         /home/name/.well-known/;
	try_files     $uri =404;
    }
    location / {
        rewrite       ^/(.*)$ https://subdomain.your_main_domain.com/$1 permanent;
    }
}
}
```

php-fpm.conf（全路径 /etc/nginx/php-fpm.conf）的内容：

``` nginx
location ~ \.php$ {
    # With php5-fpm:
    #fastcgi_pass unix:/var/run/php5-fpm.sock;
    fastcgi_pass 127.0.0.1:9000;
    fastcgi_index index.php;
    fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
    #fastcgi_param SCRIPT_FILENAME /var/www/$domain$subdomain$fastcgi_script_name;
    include fastcgi_params;
}
```

注意的问题

* Gentoo的**CONFIG_PROTECT**问题。大家都知道，对于软件的更新，除了软件包自身的更新外，还需要配置文件的更新。如果一个软件已经安装并运行，它已经在使用配置文件（这个文件有可能用户已经定制过），那么当这个软件升级后，配置文件有可能发生更新（只要和正在使用的配置文件内容不同就会更新），这个时候，机器上正在使用的配置文件和软件升级后的配置文件就可能发生冲突（如果内容不一致的话）。Gentoo定义了一个变量**CONFIG_PROTECT**（可以在**/etc/portage/make.conf**中定义）来负责保护本地的正在使用的配置文件内容，如果配置文件的路径写在**CONFIG_PROTECT**中(多个配置用空格分开，路径为全路径)，那么当配置文件对应的软件升级后，这个配置文件不会被自动升级，而是在软件升级完后系统会提示用户配置文件有更新，或者用户敲入**etc-update**命令后引导用户解决配置文件冲突问题。参考[官方文档](https://wiki.gentoo.org/wiki/CONFIG_PROTECT)
* 如果CONFIG_PROTECT="-*"表示取消配置文件保护
* 如果想让所有的配置文件都被保护，则应该这样写CONFIG_PROTECT="*"

# 更新证书
Let’s Encrypt证书的默认有效期只有**90**天，所以需要定时更新服务端的证书避免过期

一条命令更新所有服务端的证书

```bash
./letsencrypt-auto renew
```

添加为定时任务, 编辑这个文件

```bash
sudo vi /etc/cron.monthly/letsencrypt_renew
```

添加如下内容：

```bash
#!/bin/sh
/path/to/letsencrypt/letsencrypt-auto --debug renew > /var/log/letsencrypt/renew.log 2>&1
```

授予**/etc/cron.monthly/letsencrypt_renew**可执行权限

```bash
sudo chmod a+x /etc/cron.monthly/letsencrypt_renew
```

# 撤销证书
如果想收回(撤销)颁发给服务端的证书，可以使用如下命令

```bash
./letsencrypt-auto revoke --cert-path /etc/letsencrypt/live/subdomain.your_main_domain.com/cert.pem
```

# 在线HTTPS配置检查
可以使用[Jerry Qu](https://imququ.com)推荐的两个在线HTTPS配置扫描服务来检查你的网站HTTPS配置的问题，并根据建议做相应的修复。  
![](https://ssllabs.com/images/qualys-ssl-labs-logo.png)  
[Qualys SSL Labs's SSL Server Test](https://www.ssllabs.com/ssltest/index.html)  

[HTTP Security Report](https://httpsecurityreport.com)  

# 参考资料
[Let’s Encrypt官网](https://letsencrypt.org/)  
[Let's Encrypt，免费好用的 HTTPS 证书](https://imququ.com/post/letsencrypt-certificate.html)  
[LetsEncrypt SSL 证书签发(Nginx)](https://ixiaozhi.com/lets-encrypt-ssl-use-on-nginx/)  
[听说DNSpod也支持Let’s Encrypt了](https://www.qianduan.net/qie-huan-dao-lets-encryptmian-fei-zheng-shu/)  
[免费SSL安全证书Let's Encrypt安装使用教程(附Nginx/Apache配置)](http://www.vpser.net/build/letsencrypt-free-ssl.html)  
[免费SSL证书Let’s Encrypt安装使用教程:Apache和Nginx配置SSL](http://www.freehao123.com/lets-encrypt/)  
[LET’S ENCRYPT免费SSL证书申请过程](http://www.cmsky.com/lets-encrypt-ssl/)  
[Ghost Blog启用HTTPS使用LetsEncrypt SSL证书](https://yuan.ga/ghost-blog-enable-https-use-letsencrypt-ssl-certificates/)  