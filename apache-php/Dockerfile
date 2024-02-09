FROM centos:7.4.1708

RUN yum -y update

# 言語を日本語に設定
RUN localedef -i ja_JP -f UTF-8 ja_JP.UTF-8
RUN echo 'LANG="ja_JP.UTF-8"' >  /etc/locale.conf
ENV LANG ja_JP.UTF-8

# 日付を日本語に設定
RUN echo 'ZONE="Asia/Tokyo"' > /etc/sysconfig/clock
RUN rm -f /etc/localtime
RUN ln -fs /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

# 外部リポジトリ（EPEL, Remi）を追加
RUN yum -y install epel-release
RUN yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm

# apache その他 phpパッケージを導入
RUN yum -y install httpd
RUN yum -y install \
 --enablerepo=remi, \
 --enablerepo=epel,remi.,remi-php82, php php-cli php-common php-gd php-mbstring php-mysqlnd php-pdo php-xml \
 php-devel php-fpm php-gd \
 php-pear php-pecl-apcu php-soap php-xml php-xmlrpc php-pecl-zip php-intl
RUN yum -y install zip unzip patch

# 追記 docker-composerでやるので不要だった。
# ADD ./apache.conf /etc/httpd/conf/httpd.conf
# RUN chmod 644 /etc/httpd/conf/httpd.conf

# composerのインストール
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

CMD ["/usr/sbin/httpd","-D","FOREGROUND"]

WORKDIR /var/www/html