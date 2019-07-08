<!-- TOC -->

- [1. 配置环境](#1-配置环境)
    - [更新操作系统](#更新操作系统)
    - [安装 EPEL 和 Webtatic 库](#安装-epel-和-webtatic-库)
    - [安装 MySQL](#安装-mysql)
    - [MySQL补充](#mysql补充)
    - [安装 Apache](#安装-apache)
    - [安装 PHP](#安装-php)
    - [安装 Composer 并配置镜像加速](#安装-composer-并配置镜像加速)
    - [初始化 Laravel 项目](#初始化-laravel-项目)
    - [修改 Laravel 默认 MySQL 长度，适配 MySQL 5.5](#修改-laravel-默认-mysql-长度适配-mysql-55)
    - [修改 Laravel 配置文件，添加数据库信息](#修改-laravel-配置文件添加数据库信息)
    - [修改 Apache 配置文件](#修改-apache-配置文件)

<!-- /TOC -->

## 1. 配置环境

### 更新操作系统

首先，我们来更新操作系统，让我们的操作系统处于最新状态。执行如下命令。
~~~
yum update -y
~~~

### 安装 EPEL 和 Webtatic 库

接下来，我们来安装 EPEL 和 Webtatic 库，从而可以通过包管理器来安装 PHP 7。
~~~
yum install epel-release
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
~~~

### 安装 MySQL

首先，我们来安装 MySQL ，这里我们使用的是 MySQL 的一个发行版 —— MariaDB 。
~~~
yum install mariadb mariadb-server -y 
systemctl start mariadb.service
systemctl enable mariadb.service
~~~

安装完成后，执行如下命令来进行 mariadb 的初始化，并根据提示设置 root 的密码（默认密码为空）
~~~
mysql_secure_installation
~~~

### MySQL补充
查看mysql是否开启远程连接
```
show variables like '%skip_networking%';
+-----------------+-------+
| Variable_name   | Value |
+-----------------+-------+
| skip_networking | OFF   |
+-----------------+-------+
1 row in set (0.01 sec)
```
若为on则表示没开通网络连接,只能本机访问.
- [CentOS7常用环境设置](https://yq.aliyun.com/articles/175021#)
- [部署LAMP](https://help.aliyun.com/document_detail/50774.html)
- [阿里云服务器(centos7) 设置mysql账号密码开放3306端口实现远程登陆](https://www.jianshu.com/p/d0fac4648870)
- [firewall-cmd](https://wangchujiang.com/linux-command/c/firewall-cmd.html)
- [centos 7 firewall(防火墙)开放端口/删除端口/查看端口](https://blog.csdn.net/qq_36663951/article/details/82115086)
- [linux(centos 7)中配置安装apache服务器!怎么绑定域名](http://idc.wanyunshuju.com/li/6.html#%E7%BB%91%E5%AE%9A%E5%9F%9F%E5%90%8D%E9%85%8D%E7%BD%AE)

### 安装 Apache

接下来，我们来安装 Apache 作为我们的 Web 服务器。
~~~
yum install -y httpd
~~~

安装完成后，启动 Apache ，并设置默认开机自启动
~~~
systemctl start httpd.service
systemctl enable httpd.service
~~~

### 安装 PHP

接下来，我们来安装 PHP 7 ，运行php
~~~
yum install -y php70w php70w-mysql php70w-mcrypt php70w-dom php70w-mbstring
~~~

### 安装 Composer 并配置镜像加速

接下来，我们来安装 Composer ，用于后续的 php 依赖库的管理
~~~
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/bin/composer
chmod +x /usr/bin/composer
~~~

安装完成后，为 Composer 配置国内镜像源，从而提升安装依赖的速度。
~~~
composer config -g repo.packagist composer https://packagist.phpcomposer.com
~~~

### 初始化 Laravel 项目

接下来，我们来创建一个项目
~~~
cd /var/www
composer create-project laravel/laravel test
cd test
chown apache:apache -R *
~~~

这时，你可以执行 __php artisan serve --host=0.0.0.0__ 来启动 PHP 自带的服务器，并访问 http://<您的 CVM IP 地址>:8000 查看你的 Laravel 项目。

这样， 我们就完成了一个 Laravel 项目的创建。

### 修改 Laravel 默认 MySQL 长度，适配 MySQL 5.5

接下来，我们来修改 Laravel 的 Service Provider ，来确保不会出现如下问题。

修改 /var/www/test/app/Providers/AppServiceProvider.php 文件，添加如下代码

**示例代码：/var/www/test/app/Providers/AppServiceProvider.php**
~~~
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use Illuminate\Support\Facades\Schema;

class AppServiceProvider extends ServiceProvider
{

    public function boot()
    {
        Schema::defaultStringLength(191);
    }
    public function register()
    {

    }
}
~~~

### 修改 Laravel 配置文件，添加数据库信息

执行如下命令，来创建一个数据库。
~~~
mysql -uroot -p -e "create database laravel;"
~~~

然后修改 /var/www/test/.env 文件,添加数据库配置信息

**示例代码：/var/www/test/.env**
~~~
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:Wi0qQG1pXdGp5U7uM4Q2eAA2zdxTg8yM4wyo61HvL6g=
APP_DEBUG=true
APP_LOG_LEVEL=debug
APP_URL=http://localhost

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=你设置的root密码

BROADCAST_DRIVER=log
CACHE_DRIVER=file
SESSION_DRIVER=file
SESSION_LIFETIME=120
QUEUE_DRIVER=sync

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_DRIVER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_APP_CLUSTER=mt1
~~~

设置完成后，执行如下命令来生成数据库。
~~~
php /var/www/test/artisan migrate
~~~

### 修改 Apache 配置文件

接下来，我们来创建 Apache 的配置文件

- /etc/httpd/conf.d/laravel.conf

**示例代码：/etc/httpd/conf.d/laravel.conf**
~~~
<VirtualHost *:80>
    DocumentRoot "/var/www/test/public"
    ServerName <您的 CVM IP 地址>
</VirtualHost>
~~~

创建完成后，执行如下命令来重启 Apache
~~~
systemctl restart httpd.service
~~~

这样，我们就完成了 Laravel 的环境部署。