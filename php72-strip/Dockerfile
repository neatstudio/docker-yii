FROM debian:stretch-slim
MAINTAINER gouki <gouki.xiao@gmail.com>

ENV DEBIAN_FRONTEND noninteractive

WORKDIR /root

RUN \
  buildDeps='busybox curl supervisor apache2 php7.2 php7.2-opcache php7.2-mysql php7.2-sqlite3 php7.2-curl php7.2-json php7.2-mbstring php7.2-zip php7.2-xml cron libapache2-mod-php' \
  && sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list  \
  && sed -i 's/security.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list  \


  && apt-get update \
  && apt-get -y install --no-install-recommends wget apt-transport-https lsb-release ca-certificates \
  && wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg \
  && sh -c 'echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'  \
  && apt-get update \
  && apt-get -y install --no-install-recommends $buildDeps \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/cache/apt/archive/*.deb /root/.cache/composer/* \
  && a2enmod rewrite


#RUN \
#  php -r "readfile('https://getcomposer.org/installer');" > composer-setup.php  \
#  && php composer-setup.php --install-dir=/usr/local/bin --filename=composer  \
#  && php -r "unlink('composer-setup.php');"   \
#  && /usr/local/bin/composer config -g repo.packagist composer https://packagist.phpcomposer.com \
#  && /usr/local/bin/composer global require hirak/prestissimo \
#  && /usr/local/bin/composer global require "fxp/composer-asset-plugin:*" \
#  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/cache/apt/archive/*.deb /root/.cache/composer/*


VOLUME ["/var/www/html","/server/wwwroot"]

# Adding ressources
COPY 000-default.conf /etc/apache2/sites-enabled/000-default.conf
#ADD resources/* ./
COPY resources/start.sh /root/start.sh
COPY resources/run.sh   /root/run.sh
COPY resources/apache.conf /etc/supervisor/conf.d/all.conf
COPY resources/php.ini  /etc/php/7.2/apache2/php.ini

 # Give execution rights on the cron job
#COPY resources/php-cron /etc/cron.d/php-cron
#RUN chmod +x /etc/cron.d/php-cron

RUN echo "* * * * * /var/www/html/yii cron >> /var/www/html/runtime/php-cron.log 2>&1" | crontab -
CMD cron && tail -f /var/www/html/runtime/php-cron.log

RUN chmod 755 /root/*.sh

#CMD ["/usr/bin/supervisord"]

#COPY docker-entrypoint.sh /
#ENTRYPOINT ["/docker-entrypoint.sh"]

#ENTRYPOINT ["/etc/init.d/apache2", "restart"]

CMD ["/root/run.sh"]



EXPOSE 80