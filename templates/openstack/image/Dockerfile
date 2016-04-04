FROM centos:latest

RUN \
  yum clean all && \
  yum install -y epel-release && \
  yum install -y PyYAML python-jinja2 python-httplib2 python-keyczar python-paramiko \
    python-setuptools git python-pip iproute docker-engine lvm2 device-mapper

# Kolla needs v1.9.5
RUN \
  git clone http://github.com/ansible/ansible.git /opt/ansible && \
  cd /opt/ansible && \
  git checkout v1.9.5-1 && \
  git submodule update --init && \
  source ./hacking/env-setup

RUN \
  git clone https://github.com/openstack/kolla /kolla && \
  cd /kolla && \
  git checkout 2.0.0.0rc3

COPY docker.repo /etc/yum.repos.d/docker.repo
RUN pip install --upgrade docker-py

ENV PATH /opt/ansible/bin:/kolla/tools:$PATH
ENV PYTHONPATH /opt/ansible/lib
ENV ANSIBLE_LIBRARY /kolla/ansible/library

COPY globals.yml /etc/kolla/globals.yml
COPY passwords.yml /etc/kolla/passwords.yml
