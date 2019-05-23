FROM frolvlad/alpine-glibc:latest

# update Alpine with latest versions
RUN echo '@edge http://nl.alpinelinux.org/alpine/edge/main' >> /etc/apk/repositories \
  && echo '@community http://nl.alpinelinux.org/alpine/edge/community' >> /etc/apk/repositories \
  && apk update \
  && apk upgrade

# install CA certificates
RUN apk add ca-certificates

# install GCC and build tools
RUN apk add build-base \
            coreutils \
            file \
            cmake \
            git

# install tool dependencies for building RocksDB static library
RUN apk add perl \
            curl \
            bash \
            wget \
            tar \
            openssl

# install RocksDB dependencies
RUN apk add snappy snappy-dev \
            zlib zlib-dev \
            bzip2 bzip2-dev \
            lz4 lz4-dev \
            linux-headers

# install OpenJDK8
RUN apk add openjdk8 \
  && apk add java-cacerts \
  && rm /usr/lib/jvm/java-1.8-openjdk/jre/lib/security/cacerts \
  && ln -s /etc/ssl/certs/java/cacerts /usr/lib/jvm/java-1.8-openjdk/jre/lib/security/cacerts

# replace muslc JNI with OpenJDK JNI
RUN ln -s /usr/lib/jvm/java-1.8-openjdk/include/jni.h /usr/lib/gcc/x86_64-alpine-linux-musl/8.3.0/include/jni.h \
  && ln -s /usr/lib/jvm/java-1.8-openjdk/include/linux/jni_md.h  /usr/lib/gcc/x86_64-alpine-linux-musl/8.3.0/include/jni_md.h

# cleanup
RUN rm -rf /var/cache/apk/*

# gflags from source
RUN cd /tmp &&\
    git clone -b v2.0 --single-branch https://github.com/gflags/gflags.git &&\
    cd gflags &&\
    ./configure --prefix=/usr && make && make install &&\
    rm -rf /tmp/*

# zstd from source
ENV ZSTD_VER=1.4.0 ZSTD_SHA256=63be339137d2b683c6d19a9e34f4fb684790e864fee13c7dd40e197a64c705c1 ZSTD_DOWNLOAD_BASE=https://github.com/facebook/zstd/archive
RUN cd /tmp &&\
    curl -o zstd-${ZSTD_VER}.tar.gz -L ${ZSTD_DOWNLOAD_BASE}/v${ZSTD_VER}.tar.gz &&\
    ZSTD_SHA256_ACTUAL=`openssl sha256 -r zstd-${ZSTD_VER}.tar.gz | cut -d ' ' -f 1` &&\
    if [ "$ZSTD_SHA256" != "$ZSTD_SHA256_ACTUAL" ]; then \
        echo zstd-${ZSTD_VER}.tar.gz checksum mismatch, expected=\"$ZSTD_SHA256\" actual=\"$ZSTD_SHA256_ACTUAL\"; \
        exit 1; \
    fi &&\
    tar xvzf zstd-${ZSTD_VER}.tar.gz &&\
    cd zstd-${ZSTD_VER}/lib && make PREFIX=/usr CFLAGS='-fPIC -O2' all install &&\
    cd / && rm -rf /tmp/*

# puts javac in the PATH
ENV PATH=/usr/lib/jvm/java-1.8-openjdk/bin:$PATH
