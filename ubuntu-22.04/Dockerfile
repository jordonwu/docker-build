# This dockerfile uses the ubuntu image
# VERSION 0 - EDITION 1
# Author:  Yen-Chin, Lee <yenchin@weintek.com>
# Command format: Instruction [arguments / command] ..

FROM ubuntu:22.04
#MAINTAINER Yen-Chin, Lee, coldnew.tw@gmail.com
LABEL maintainer="Yen-Chin, Lee, coldnew.tw@gmail.com"
LABEL maintainer="Jordon Wu, jordon.wu@gmail.com"

# Disable interactive functions
ENV DEBIAN_FRONTEND=noninteractive

# Using mirrors for apt
### RUN \
###  mv /etc/apt/sources.list /etc/apt/sources.list.bak && \
###  echo "deb http://mirrors.aliyun.com/ubuntu/ jammy main restricted" > /etc/apt/sources.list && \
###  echo "deb http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted" >> /etc/apt/sources.list && \
###  echo "deb http://mirrors.aliyun.com/ubuntu/ jammy universe" >> /etc/apt/sources.list && \
###  echo "deb http://mirrors.aliyun.com/ubuntu/ jammy-updates universe" >> /etc/apt/sources.list && \
###  echo "deb http://mirrors.aliyun.com/ubuntu/ jammy multiverse" >> /etc/apt/sources.list && \
###  echo "deb http://mirrors.aliyun.com/ubuntu/ jammy-updates multiverse" >> /etc/apt/sources.list && \
###  echo "deb http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse" >> /etc/apt/sources.list && \
###  echo "deb http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted" >> /etc/apt/sources.list && \
###  echo "deb http://mirrors.aliyun.com/ubuntu/ jammy-security universe" >> /etc/apt/sources.list && \
###  echo "deb http://mirrors.aliyun.com/ubuntu/ jammy-security multiverse" >> /etc/apt/sources.list
###

RUN \
 mv /etc/apt/sources.list /etc/apt/sources.list.bak && \
 echo "deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted" > /etc/apt/sources.list && \
 echo "deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted" >> /etc/apt/sources.list && \
 echo "deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy universe" >> /etc/apt/sources.list && \
 echo "deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates universe" >> /etc/apt/sources.list && \
 echo "deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy multiverse" >> /etc/apt/sources.list && \
 echo "deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates multiverse" >> /etc/apt/sources.list && \
 echo "deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse" >> /etc/apt/sources.list && \
 echo "deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted" >> /etc/apt/sources.list && \
 echo "deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security universe" >> /etc/apt/sources.list && \
 echo "deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security multiverse" >> /etc/apt/sources.list

RUN cat /etc/apt/sources.list

# Add 32bit package in package list
RUN dpkg --add-architecture i386
#RUN sed -Ei 's/^# deb-src /deb-src /' /etc/apt/sources.list

# Update package infos first
RUN apt-get update -y

## Install requred packages:
# http://www.yoctoproject.org/docs/current/ref-manual/ref-manual.html
RUN apt-get install -y curl
### RUN apt-get -y build-dep qemu curl && \
###     apt-get -y remove oss4-dev
###
# Essentials
### RUN apt-get install -y gawk wget git-core diffstat unzip texinfo gcc-multilib \
###      build-essential chrpath socat tar  python3 python3-pip python3-pexpect \
###      debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev \
###      xterm cpio vim
###

RUN apt install -y gawk wget git-core diffstat unzip texinfo \
    gcc-multilib build-essential chrpath socat file cpio python3 \
    python3-pip python3-pexpect xz-utils debianutils iputils-ping \
    libsdl1.2-dev xterm tar locales net-tools rsync sudo vim curl zstd \
    liblz4-tool libssl-dev bc lzop libgnutls28-dev efitools git-lfs

### # Graphical and Eclipse Plug-In Extras
### RUN apt-get install -y libsdl1.2-dev xterm
###
### # Documentation
### RUN apt-get install -y make xsltproc docbook-utils fop dblatex xmlto

# OpenEmbedded Self-Test
#RUN apt-get install -y python-git

# Extra package for build with NXP's images
### RUN apt-get install -y \
###     sed cvs subversion coreutils texi2html \
###     help2man  gcc g++ \
###     desktop-file-utils libgl1-mesa-dev libglu1-mesa-dev mercurial \
###     autoconf automake groff curl lzop asciidoc u-boot-tools busybox
###
# Extra package for Xilinx PetaLinux
# RUN apt-get install -y xvfb libtool libncurses5-dev libssl-dev zlib1g-dev:i386 tftpd

# Install repo tool for some bsp case, like NXP's yocto
RUN curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > /usr/bin/repo
RUN chmod a+x /usr/bin/repo

### # Install Java
### #RUN \
### #  echo oracle-java11-installer shared/accepted-oracle-license-v1-2 select true | debconf-set-selections && \
### #  apt-get install -y software-properties-common && \
### #  add-apt-repository -y ppa:linuxuprising/java && \
### #  apt-get update && \
### #  apt-get install -y oracle-java11-installer-local && \
### #  rm -rf /var/lib/apt/lists/* && \
### #  rm -rf /var/cache/oracle-jdk11-installer-local

# Set the locale, else yocto will complain
RUN apt-get install locales -y
RUN locale-gen en_US.UTF-8 && \
	DEBIAN_FRONTEND=noninteractive dpkg-reconfigure locales
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8

# make /bin/sh symlink to bash instead of dash
# Xilinx's petalinux need this
RUN echo "dash dash/sh boolean false" | debconf-set-selections
RUN DEBIAN_FRONTEND=noninteractive dpkg-reconfigure dash

# default workdir is /docker_build
WORKDIR /docker_build

# Add entry point, we use entrypoint.sh to mapping host user to
# container
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
