FROM php:7.3.7-fpm
MAINTAINER zhaofantian

# 更新为国内镜像
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak
COPY sources.list /etc/apt/sources.list

RUN docker-php-ext-install -j$(nproc) bcmath calendar exif gettext sockets dba mysqli pcntl pdo_mysql shmop sysvmsg sysvsem sysvshm iconv
# GD 扩展
RUN apt-get update --fix-missing && apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
    && rm -r /var/lib/apt/lists/* \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd
# imagick 扩展
RUN export CFLAGS="$PHP_CFLAGS" CPPFLAGS="$PHP_CPPFLAGS" LDFLAGS="$PHP_LDFLAGS" \
    && apt-get update --fix-missing && apt-get install -y libmagickwand-dev \
    && rm -r /var/lib/apt/lists/* \
    && pecl install imagick-3.4.4 \
    && docker-php-ext-enable imagick
# mcrypt 扩展
RUN apt-get update --fix-missing && apt-get install -y libmcrypt-dev \
    && rm -r /var/lib/apt/lists/* \
    && pecl install mcrypt-1.0.2 \
    && docker-php-ext-enable mcrypt
# Memcached 扩展
RUN apt-get update --fix-missing && apt-get install -y libmemcached-dev zlib1g-dev \
    && rm -r /var/lib/apt/lists/* \
    && pecl install memcached-3.1.3 \
    && docker-php-ext-enable memcached
# zip
RUN apt-get update --fix-missing && apt-get install -y libzip-dev \
    && rm -r /var/lib/apt/lists/* \
    && docker-php-ext-install -j$(nproc) zip

# redis 扩展
RUN pecl install redis-5.0.0 && docker-php-ext-enable redis
# opcache 扩展
RUN docker-php-ext-configure opcache --enable-opcache && docker-php-ext-install opcache
# xdebug 扩展
RUN pecl install xdebug-2.7.2 && docker-php-ext-enable xdebug
# swoole 扩展
#RUN pecl install swoole-4.4.0 && docker-php-ext-enable swoole

# 安装cron
RUN apt-get update --fix-missing && apt-get install -y cron

# 安装vim
RUN apt-get update --fix-missing && apt-get install -y vim

# 暴露9000端口
EXPOSE 9000


