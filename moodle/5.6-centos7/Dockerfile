FROM moodletw/httpd:5.6-centos7
MAINTAINER Click-AP

ENV TZ=Asia/Taipei
ENV PHPVER = 56

USER root

#Install SVN client from official repository
RUN echo $'[WandiscoSVN] \n\
name=Wandisco SVN Repo \n\
baseurl=http://opensource.wandisco.com/centos/7/svn-1.9/RPMS/$basearch/ \n\
enabled=1 \n\
gpgcheck=0 ' > /etc/yum.repos.d/wandisco-svn.repo

#RUN cat /etc/yum.repos.d/wandisco-svn.repo

RUN yum update -y \
 && yum install -y subversion \
 && yum clean all

RUN svn --version

ADD ./entrypoint /usr/local/entrypoint
RUN chmod -v +x /usr/local/entrypoint

VOLUME ["/var/www/moodledata"]

USER docker

EXPOSE 80
WORKDIR /var/www/html
ENTRYPOINT ["/tini", "--", "/usr/local/entrypoint"]
