FROM moodletw/core:centos7
MAINTAINER Click-AP

ENV TZ=Asia/Taipei
ENV PHPVER = 56

RUN yum update -y && \
 yum -y install httpd bzip2 && \
 yum -y install yum-utils && \
 yum-config-manager --enable remi-php56 && \
 yum -y install php php-mysql php-gd php-pear php-xml php-xmlrpc php-mbstring php-soap php-intl php-opcache php-fileinfo php-mcrypt php-cli php-zip php-ldap \
 && yum clean all

RUN sed -i \
  -e "s|;date.timezone =|date.timezone = 'Asia/Taipei'|g" \
  -e 's/post_max_size = 8M/post_max_size = 1024M/g' \
  -e 's/upload_max_filesize = 2M/upload_max_filesize = 1024M/g' \
  -e 's/max_execution_time = 30/max_execution_time = 600/g' \
  -e 's/max_input_time = 60/max_input_time = 600/g' \
  -e 's/memory_limit = 128M/memory_limit = 1024M/g' /etc/php.ini

ADD http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.bz2 /tmp/

RUN tar xvjfC /tmp/ioncube_loaders_lin_x86-64.tar.bz2 /tmp/ && \
 cp /tmp/ioncube/ioncube_loader_lin_5.6.so /usr/lib64/php/modules/ && \
 rm /tmp/ioncube_loaders_lin_x86-64.tar.bz2 && \
 rm -rf /tmp/ioncube
COPY 00-ioncube.ini /etc/php.d/00-ioncube.ini

# Simple startup script to avoid some issues observed with container restart
ADD ./entrypoint /usr/local/entrypoint
RUN chmod -v +x /usr/local/entrypoint

VOLUME ["/var/www/moodledata"]

USER docker

EXPOSE 80
WORKDIR /var/www/html
ENTRYPOINT ["/tini", "--", "/usr/local/entrypoint"]