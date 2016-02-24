---
layout: post
title:  "Protocol buffer on php"
subtitle:   ""  
date:       2016-02-24
author:     "figo"
header-img: ""
tags:
    - php
    - protobuf
    - protocol buffer
    - composer
    - 2016
---
First, install protobuf from source
{% highlight bash %}
$ git clone https://github.com/google/protobuf.git
$ cd protobuf
$ ./autogen.sh
$ ./configure
$ make
$ make check
$ sudo make install
$ sudo ldconfig # refresh shared library cache.
{% endhighlight %}
Second, install php-protocolbuffers
{% highlight bash %}
$ git clone https://github.com/chobie/php-protocolbuffers.git
$ cd php-protocolbuffers
$ phpize
$ ./configure
$ make
$ sudo make install
{% endhighlight %}
please add following line to your php.ini
{% highlight bash %}
$ sudo vi /etc/php/fpm-php5.6/ext/protocolbuffers.ini
extension=protocolbuffers.so
$ sudo vi /etc/php/cli-php5.6/ext/protocolbuffers.ini
extension=protocolbuffers.so
$ sudo ln -s /etc/php/fpm-php5.6/ext/protocolbuffers.ini /etc/php/fpm-php5.6/ext-active/protocolbuffers.ini
$ sudo ln -s /etc/php/cli-php5.6/ext/protocolbuffers.ini /etc/php/cli-php5.6/ext-active/protocolbuffers.ini
{% endhighlight %}
Third, install composer.phar
{% highlight bash %}
$ curl -s http://getcomposer.org/installer | php
$ sudo mv composer.phar /usr/local/bin
{% endhighlight %}
Fourth, install protoc-gen-php
{% highlight bash %}
$ git clone https://github.com/chobie/protoc-gen-php.git
$ cd https://github.com/chobie/protoc-gen-php.git
{% endhighlight %}
add protocolbuffers/protoc-gen-php entry to your global composer.json ($HOME/.composer/composer.json)
{% highlight bash %}
{
"require": {
"protocolbuffers/protoc-gen-php": "dev-master"
}
}
{% endhighlight %}
install with composer
{% highlight bash %}
$ composer.phar global install
{% endhighlight %}
set PATH (add this line to your .bashrc or .zshrc.)
{% highlight bash %}
$ export PATH=$HOME/.composer/vendor/bin/:$PATH
{% endhighlight %}
Fifth, just make a demo
demo .proto file definition
{% highlight bash %}
package openviewmobile;
message Person
{
required int32 user_id = 1;
optional string name = 2;
}
{% endhighlight %}
okay, let’s generate php classes with protoc
{% highlight bash %}
$ protoc --php_out . person.proto
{% endhighlight %}
when you want to use openviewmobile\Person class, you just do require person.proto.php.
{% highlight php %}
setUserId(1234);
$person->setName("chobie");
$raw = $person->serializeToString();
echo $raw
?>
{% endhighlight %}
[Google Protocol Buffers Official Doc][1]  
[php-protocolbuffers wiki][2]  
[Protocol Buffers – Google’s data interchange format][3]

[1]: https://developers.google.com/protocol-buffers/
[2]: https://github.com/chobie/php-protocolbuffers/wiki
[3]: https://github.com/google/protobuf
