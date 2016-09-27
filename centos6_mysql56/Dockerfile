FROM centos:centos6
MAINTAINER kzf3 <growth.man@gmail.com>


# initial setting
RUN yum -y update && yum -y install epel-release \
    wget \
    yum-utils

# MySQL Server
RUN yum -y localinstall http://dev.mysql.com/get/mysql57-community-release-el6-9.noarch.rpm && \
    yum repolist enabled | grep "mysql.*-community.*"  && \
    yum-config-manager --disable mysql57-community && \
    yum-config-manager --enable mysql56-community && \
    yum -y install mysql-community-server && \
    mkdir /var/log/mysql

ADD my.cnf /etc/my.cnf

# localtime
RUN cp /etc/localtime /etc/localtime.bk && \
    cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

# japanese
RUN yum -y groupinstall "Japanese Support" && \
    sed -i -e s/en_US/ja_JP/g /etc/sysconfig/i18n && \
    localedef -f UTF-8 -i ja_JP ja_JP.utf8 && \
    source /etc/sysconfig/i18n

EXPOSE 3306
CMD ["/usr/bin/mysqld_safe"]
