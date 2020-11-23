# THIS DOES NOT WORK

FROM ubuntu:18.04

RUN apt-get -y update && \
  apt-get install -y \
    libmicrohttpd-dev \
    libjansson-dev \
    libssl-dev \
    libsofia-sip-ua-dev \
    libglib2.0-dev \
    libopus-dev \
    libogg-dev \
    libcurl4-openssl-dev \
    liblua5.3-dev \
    libconfig-dev \
    libnanomsg-dev \
    pkg-config \
    gengetopt \
    libtool \
    automake \
    build-essential \
    wget \
    git \
    python3 \
    python3-pip \
    python3-setuptools \
    python3-wheel \
    ninja-build \
    gtk-doc-tools && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

ENV PATH "$PATH:/usr/local/bin"

RUN cd /tmp && \
  wget https://github.com/cisco/libsrtp/archive/v2.3.0.tar.gz && \
  tar xfv v2.3.0.tar.gz && \
  cd libsrtp-2.3.0 && \
  ./configure --prefix=/usr --enable-openssl && \
  make shared_library && \
  make install

RUN cd /tmp && \
  git clone https://gitlab.freedesktop.org/libnice/libnice && \
  cd libnice && \
  meson builddir && meson compile

RUN cd /tmp && \
  git clone https://github.com/sctplab/usrsctp && \
  cd usrsctp && \
  ./bootstrap && \
  ./configure --prefix=/usr && make && make install

RUN cd /tmp && \
  git clone https://github.com/warmcat/libwebsockets.git \
  cd libwebsockets \
  mkdir build \
  cd build \
  cmake -DLWS_MAX_SMP=1 -DCMAKE_INSTALL_PREFIX:PATH=/usr -DCMAKE_C_FLAGS="-fpic" .. \
  make && sudo make install

RUN cd /tmp && \
  git clone https://github.com/eclipse/paho.mqtt.c.git \
  cd paho.mqtt.c \
  sudo prefix=/usr make install

RUN cd /tmp && \
  git clone https://github.com/alanxz/rabbitmq-c \
  cd rabbitmq-c \
  git submodule init \
  git submodule update \
  mkdir build && cd build \
  cmake -DCMAKE_INSTALL_PREFIX=/usr .. \
  make && sudo make install

RUN git clone https://github.com/meetecho/janus-gateway.git && \
  cd janus-gateway && \
  sh autogen.sh && \
  ./configure --prefix=/usr/local && \
  make && \
  make install && \
  make configs
