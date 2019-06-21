ARG TINI_VERSION=v0.16.1
FROM canvastw/tini:${TINI_VERSION} as tini

FROM centos:7
MAINTAINER Click-AP

## Create a 'docker' user
RUN  groupadd --gid 9999 docker \
  && useradd --uid 9999 --gid 9999 --system -l --comment "Docker User" docker \
  && usermod -L docker

## Upgrade all packages
## Install common tools
RUN yum update -y \
 && yum -y install wget && \
 && yum -y install epel-release \
 && yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm \
 && yum clean all

# Install Tini for init use (reaps defunct processes and forwards signals)
COPY --from=tini /tini /tini

# Switch to the 'docker' user
USER root