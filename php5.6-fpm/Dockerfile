FROM php:5.6.4-fpm
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
# mcrypt 扩展
RUN apt-get update --fix-missing && apt-get install -y libmcrypt-dev \
    && rm -r /var/lib/apt/lists/* \
    && pecl install mcrypt-1.0.2 \
    && docker-php-ext-enable mcrypt
# zip
RUN apt-get update --fix-missing && apt-get install -y libzip-dev \
    && rm -r /var/lib/apt/lists/* \
    && docker-php-ext-install -j$(nproc) zip
# 安装cron
RUN apt-get update --fix-missing && apt-get install -y cron
# 安装vim
RUN apt-get update --fix-missing && apt-get install -y vim
# 暴露9000端口
EXPOSE 9000