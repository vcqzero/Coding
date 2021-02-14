###  安装

安装必要扩展

```shell
sudo apt install \
build-essential \
pkg-config \
libxml2 \
libxml2-dev \
sqlite3 \
libsqlite3-dev \
autoconf \
zlib1g \
libssl-dev \
libcurl4-openssl-dev \
libjpeg-dev \
libpng-dev \
libxpm-dev \
bzip2 \
openssl \
curl \
libtool \
libzip-dev

```

编译安装

```shell
./configure --prefix=/mnt/d/php/php-7.4.15 \
--enable-fpm \
--with-openssl \
--enable-bcmath \
--with-curl \
--enable-ftp \
--enable-zip \
--enable-gd \
--enable-mbstring \
--enable-sockets \
--enable-pcntl \
--with-zlib \
--enable-mysqlnd \
--with-pdo-mysql=mysqlnd
make && make install
```

php-fpm配置

```shell
# 在etc目录下
cp php-fpm.conf.default php-fpm.conf
cp php-fpm.d/www.conf.default www.conf
```

php.ini配置

```shell
# 将php.ini复制到lib文件下
```

启动/关闭

```shell
# 启动
./sbin/php-fpm # 或者将sbin放到PATH中
# 关闭
pkill php-fpm 
# 查看进程
netstat -lntp
ps -ef | grep php
```

php扩展

```shell
# 查看已安装扩展
php -m
# 安装扩展
phpize
./configure --with-php-config=/mnt/d/php/php-7.4.15/bin/php-config
make && make install
```

### composer

```shell
# 安装
curl -sS https://getcomposer.org/installer | php -- --install-dir=/mnt/d/php/composer --filename=composer
# 国内镜像
composer config -g repo.packagist composer https://packagist.phpcomposer.com

# 降级
composer self-update --1
```



### 报错

#### No package 'oniguruma' found

```shell
wget https://github.com/kkos/oniguruma/archive/v6.9.4.tar.gz -O oniguruma-6.9.4.tar.gz 
tar -zxvf oniguruma-6.9.4.tar.gzcd oniguruma-6.9.4/
./autogen.sh
./configure
make
sudo make install

## 如果报libtool错误
sudo apt install libtool
```

