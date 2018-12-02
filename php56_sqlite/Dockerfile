FROM debian:jessie-slim
LABEL maintainer="gouki.xiao@gmail.com"

ENV DEBIAN_FRONTEND noninteractive

WORKDIR /root

RUN \
  buildDeps='busybox git curl supervisor apache2 php5 php5-common php5-mysqlnd php5-sqlite php5-curl php5-gd php5-mcrypt libapache2-mod-php5' \
  && sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list  \
  && apt-get update \
  && apt-get -y install --no-install-recommends wget apt-transport-https lsb-release ca-certificates $buildDeps \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/cache/apt/archive/*.deb \
  && a2enmod rewrite

RUN \
  php -r "readfile('https://getcomposer.org/installer');" > composer-setup.php  \
  && php composer-setup.php --install-dir=/usr/local/bin --filename=composer  \
  && php -r "unlink('composer-setup.php');"   \
  && /usr/local/bin/composer config -g repo.packagist composer https://packagist.phpcomposer.com \
  && /usr/local/bin/composer global require hirak/prestissimo \
  && /usr/local/bin/composer global require "fxp/composer-asset-plugin:*" \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/cache/apt/archive/*.deb /root/.cache/composer/*


VOLUME ["/var/www/html","/server/wwwroot"]

# Adding ressources
#COPY 000-default.conf /etc/apache2/sites-enabled/000-default.conf
#COPY resources/start.sh /root/start.sh
#COPY resources/run.sh   /root/run.sh
#COPY resources/apache.conf /etc/supervisor/conf.d/all.conf

# Adding ressources
COPY 000-default.conf /etc/apache2/sites-enabled/000-default.conf
COPY resources/start.sh /root/start.sh
COPY resources/run.sh   /root/run.sh
COPY resources/apache.conf /etc/supervisor/conf.d/all.conf
COPY resources/php.ini  /etc/php5/apache2/php.ini

#ADD resources/* ./
RUN chmod 755 /root/*.sh
CMD ["/root/run.sh"]

EXPOSE 80