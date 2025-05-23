FROM ubuntu:24.10

LABEL maintainer="Nadezhda Ryabtsova <nadezhdaryabtsova@gmail.com>"
LABEL description="Provides an image with Janus Gateway"

RUN echo "MIRRORSITE=http://archive.ubuntu.com/ubuntu" >> /etc/pbuilderrc && \
    apt-get update && \
	apt-get install -y --no-install-recommends \
		git \
		build-essential \
		autoconf \
		automake \
		autotools-dev \
		dh-make \
		debhelper \
		devscripts \
		fakeroot \
		xutils \
		lintian \
		pbuilder \
		libconfig-dev \
		libmicrohttpd-dev \
		libjansson-dev \
		libnice-dev \
		libssl-dev \
		libsrtp2-dev \
		libsofia-sip-ua-dev \
		libglib2.0-dev \
		libopus-dev \
		libogg-dev \
		pkg-config \
		gengetopt \
		libtool \
		automake \
		cmake \
		libmicrohttpd-dev \
		libjansson-dev \
		libcurl4-openssl-dev \
		libglib2.0-dev \
		libssl-dev \
		libopus-dev \
		libavutil-dev \
		libavcodec-dev \
		libavformat-dev \
		libwebsockets-dev \
		liblua5.3-dev \
        libsrtp2-dev && \
	rm -rf /var/lib/apt/lists/*

# Build and install RNNoise
RUN cd /tmp && \
    git clone https://github.com/xiph/rnnoise.git && \
    cd rnnoise && \
    git checkout v0.2 && \
    ./autogen.sh && \
    ./configure && \
    make && \
    make install && \
    ldconfig && \
    cd .. && \
    rm -rf rnnoise

RUN mkdir -p /usr/src/janus /var/janus/janus/log /var/janus/janus/data && \
	cd /usr/src/janus && \
	wget -c https://github.com/meetecho/janus-gateway/archive/v1.3.1.tar.gz && \
	tar -xzf v1.3.1.tar.gz && \
	cd janus-gateway-1.3.1 && \
	sh autogen.sh && \
	./configure --prefix=/var/janus/janus --enable-post-processing --disable-rabbitmq --disable-data-channels --disable-aes-gcm && \
	make && make install && make configs && \
	rm -rf /usr/src/janus

EXPOSE 8088/tcp 8188/tcp 8188/udp

ENTRYPOINT ["/var/janus/janus/bin/janus"]
CMD []
