FROM debian:stretch-slim
MAINTAINER gouki <gouki.xiao@gmail.com>

ENV DEBIAN_FRONTEND noninteractive

WORKDIR /root

RUN \
   sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list  \
   && apt-get update \
   && apt-get install -y --no-install-recommends wget apt-transport-https mysql-server \
   && apt-get clean \
   && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/cache/apt/archive/*.deb

RUN \
   service mysql start

