

# PHP Docker容器

## 安装工具

```shell
# php-fpm容器非常精简，如果在使用者需要一些工具，需要自己安装

apt-get install -y vim # 安装vim
apt-get install -y procps # 安装ps工具(推荐安装，可管理php-fpm进程)
```

## 管理php-fpm

```shell
# 启动php-fpm

# 在后台运行php-fpm
php-fpm -D 

# 关闭
# 查找php-fpm master主进程
ps -ef | grep php-fpm 
kill all MASTER_PID

```

## Composer

```
# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
```

它对我有用，可以通过访问我的容器bash并执行以下命令来测试是否安装了 Composer :

```shell
composer --version
Composer version 1.6.5 2018-05-04 11:44:59
```

## Laravel 服务

可以在docker内启动php服务

```shel
php artisan serve --host 0.0.0.0 --port 8000
```

这样可以在docker外部通过8000端口，访问PHP服务；

##  完整的Dockfile

```shell
# Dockerfile
FROM php:7.4-fpm

COPY php/php.ini-development /usr/local/etc/php/php.ini
COPY php/phpinfo.php /phpinfo.php

# Install Zip Vim Ps
RUN apt-get update && apt-get install -y zip \
    && apt-get install -y vim \
    && apt-get install -y procps 

# 安装nginx
RUN apt-get update && apt-get install -y nginx
# gd
RUN apt-get update && apt-get install -y \ 
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd 
RUN docker-php-ext-install -j$(nproc) pdo_mysql
# bcmath
RUN docker-php-ext-install -j$(nproc) bcmath

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer 
    # composer config repo.packagist composer https://packagist.phpcomposer.com

RUN mv /phpinfo.php /var/www/html/phpinfo.php
```

## 

