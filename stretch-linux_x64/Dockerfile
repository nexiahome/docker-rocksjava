FROM openjdk:8-jdk-stretch

RUN apt-get update && apt-get install -y --no-install-recommends \
		build-essential \
		git \
		cmake \
		openssl \
		libsnappy-dev \
		zlib1g-dev \
		libbz2-dev \
		liblz4-dev \
		libzstd-dev \
	&& rm -rf /var/lib/apt/lists/*

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
