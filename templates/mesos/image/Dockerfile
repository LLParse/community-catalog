FROM centos:7


RUN yum install -y tar wget git

RUN wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo

# Install the EPEL repo so that we can pull in 'libserf-1' as part of our
# subversion install below.
RUN yum install -y epel-release

ADD wandisco-svn.repo /etc/yum.repos.d/wandisco-svn.repo

# Parts of Mesos require systemd in order to operate. However, Mesos
# only supports versions of systemd that contain the 'Delegate' flag.
# This flag was first introduced in 'systemd version 218', which is
# lower than the default version installed by centos. Luckily, centos
# 7.1 has a patched 'systemd < 218' that contains the 'Delegate' flag.
# Explicity update systemd to this patched version.
RUN yum update -y systemd

# Install essential development tools.
RUN yum groupinstall -y "Development Tools"

# Install other Mesos dependencies.
RUN yum install -y apache-maven python-devel java-1.8.0-openjdk-devel zlib-devel libcurl-devel openssl-devel cyrus-sasl-devel cyrus-sasl-md5 apr-devel subversion-devel apr-util-devel

RUN \
  curl -LO https://dist.apache.org/repos/dist/release/mesos/0.28.0/mesos-0.28.0.tar.gz && \
  tar xzf mesos-0.28.0.tar.gz && \
  rm -f mesos-0.28.0.tar.gz && \
  mv mesos-0.28.0 mesos

RUN \
  mkdir /mesos/build && \
  cd /mesos/build && \
  ../configure && \
  make
