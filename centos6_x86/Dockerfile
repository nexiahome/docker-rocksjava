FROM i386/centos:6

MAINTAINER Evolved Binary Ltd <tech@evolvedbinary.com>

LABEL name="CentOS 6 (x86) RocksJava Build Environment" \
  vendor="Evolved Binary Ltd"

RUN curl http://mirror.centos.org/centos/6/os/i386/Packages/udev-147-2.73.el6_8.2.i686.rpm --output udev-147-2.73.el6_8.2.i686.rpm \
  && rpm -Uvh udev-147-2.73.el6_8.2.i686.rpm --nodeps \
  && curl --output util-linux-ng-2.17.2-12.28.el6_9.2.i686.rpm http://mirror.centos.org/centos/6/os/i386/Packages/util-linux-ng-2.17.2-12.28.el6_9.2.i686.rpm \
  && rpm -Uvh util-linux-ng-2.17.2-12.28.el6_9.2.i686.rpm \
  && linux32 yum --setopt=tsflags=nodocs -y reinstall udev.i686 util-linux-ng.i686 \
  && linux32 yum --setopt=tsflags=nodocs -y update \
  && linux32 yum --setopt=tsflags=nodocs -y install epel-release \
  && linux32 yum --setopt=tsflags=nodocs -y install wget tar which \
  && wget -O /etc/yum.repos.d/devtools-2.repo https://people.centos.org/tru/devtools-2/devtools-2.repo \
  && linux32 yum --setopt=tsflags=nodocs -y install make perl devtoolset-2-binutils devtoolset-2-gcc devtoolset-2-gcc-c++ cmake3 \
  && linux32 yum --setopt=tsflags=nodocs -y install jemalloc-devel zlib-devel bzip2-devel lz4-devel snappy-devel libzstd-devel \
  && linux32 yum --setopt=tsflags=nodocs -y install openssl java-1.7.0-openjdk-devel \
  && linux32 yum -y clean all \
  && alternatives --install /usr/local/bin/cmake cmake /usr/bin/cmake 10 --slave /usr/local/bin/ctest ctest /usr/bin/ctest --slave /usr/local/bin/cpack cpack /usr/bin/cpack --slave /usr/local/bin/ccmake ccmake /usr/bin/ccmake \
  && alternatives --install /usr/local/bin/cmake cmake /usr/bin/cmake3 20 --slave /usr/local/bin/ctest ctest /usr/bin/ctest3 --slave /usr/local/bin/cpack cpack /usr/bin/cpack3 --slave /usr/local/bin/ccmake ccmake /usr/bin/ccmake3 \
  && wget https://github.com/gflags/gflags/archive/v2.0.tar.gz -O gflags-2.0.tar.gz \
  && tar xvfz gflags-2.0.tar.gz; cd gflags-2.0; linux32 scl enable devtoolset-2 ./configure; linux32 scl enable devtoolset-2 make; linux32 scl enable devtoolset-2 'make install'; cd ..; rm -r gflags-2.0*

ENV JAVA_HOME=/usr/lib/jvm/java-1.7.0
ENV PATH=$JAVA_HOME:$PATH
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib
