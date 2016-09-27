FROM centos:centos6
MAINTAINER kzf3 <growth.man@gmail.com>

RUN yum -y update && yum -y install epel-release \
    yum-utils

# Apache
RUN yum -y install httpd

# PHP
ADD http://rpms.famillecollet.com/enterprise/remi-release-6.rpm /root/
RUN rpm -Uvh /root/remi-release-6.rpm && rm -rf /root/remi-release-6.rpm
RUN yum install -y --enablerepo=remi,remi-php56 \
	php \
	php-mbstring \
	php-mysql \
	php-devel \
	php-pecl-xdebug \
	php-opcache \
	php-pecl-apcu \
	php-xml \
	php-gd \
	php-phpunit-PHPUnit

# Git
RUN yum -y install git \
	make \
	libcurl-devel \
	gcc \
	openssl-devel \
	expat-devel \
	gettext \
	asciidoc \
	xmlto \
	perl-ExtUtils-MakeMaker

RUN git clone https://github.com/git/git.git /root/git
RUN cd /root/git/ && \
    make configure && \
    ./configure --prefix=/usr/local --with-curl --with-expat && \
    make all doc && \
    make install install-doc install-html

# MySQL Client
RUN yum -y localinstall http://dev.mysql.com/get/mysql57-community-release-el6-9.noarch.rpm && \
    yum repolist enabled | grep "mysql.*-community.*"  && \
    yum-config-manager --disable mysql57-community && \
    yum-config-manager --enable mysql56-community && \
    yum -y install mysql-community-client && \
    mkdir /var/log/mysql

# localtime
RUN cp /etc/localtime /etc/localtime.bk && \
    cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

# japanese
RUN yum -y groupinstall "Japanese Support" && \
    sed -i -e s/en_US/ja_JP/g /etc/sysconfig/i18n && \
    localedef -f UTF-8 -i ja_JP ja_JP.utf8 && \
    source /etc/sysconfig/i18n

EXPOSE 80
