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
    libnice-dev \
    librabbitmq-dev \
    nginx \
    vim-tiny \
    gtk-doc-tools && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

RUN cd /tmp && \
  wget https://github.com/Kitware/CMake/releases/download/v3.16.5/cmake-3.16.5.tar.gz && \
  tar -zxvf cmake-3.16.5.tar.gz && \
  cd cmake-3.16.5 && \
  ./bootstrap && \
  make && \
  make install

RUN cd /tmp && \
  wget https://github.com/cisco/libsrtp/archive/v2.3.0.tar.gz && \
  tar xfv v2.3.0.tar.gz && \
  cd libsrtp-2.3.0 && \
  ./configure --prefix=/usr --enable-openssl && \
  make shared_library && \
  make install

RUN cd /tmp && \
  git clone https://github.com/sctplab/usrsctp && \
  cd usrsctp && \
  ./bootstrap && \
  ./configure --prefix=/usr && make && make install

RUN cd /tmp && \
  git clone https://github.com/warmcat/libwebsockets.git && \
  cd libwebsockets && \
  mkdir build && \
  cd build && \
  cmake -DLWS_MAX_SMP=1 -DCMAKE_INSTALL_PREFIX:PATH=/usr -DCMAKE_C_FLAGS="-fpic" .. && \
  make && make install

RUN cd /tmp && \
  git clone https://github.com/eclipse/paho.mqtt.c.git && \
  cd paho.mqtt.c && \
  prefix=/usr make install

RUN git clone https://github.com/meetecho/janus-gateway.git && \
  cd janus-gateway && \
  sh autogen.sh && \
  ./configure --prefix=/opt/janus --disable-rabbitmq --disable-mqtt && \
  make && \
  make install && \
  make configs

COPY nginx/nginx.conf /etc/nginx/nginx.conf

EXPOSE 80 7088 8088 8188 8089
EXPOSE 10000-10200/udp

CMD ["nginx", "-g", "daemon off;"]

RUN cd /opt/janus/bin && \
  ./janus -F /opt/janus/etc/janus --daemon -L logfile.txt
