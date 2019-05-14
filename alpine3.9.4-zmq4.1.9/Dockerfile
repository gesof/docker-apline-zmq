FROM alpine:3.9.4
MAINTAINER gesof <office@acron.ro>

RUN apk add --no-cache --virtual .persistent-deps \
    curl \
    g++ \
    make \
    automake \
    autoconf \
    libtool \
    util-linux-dev \
    git

#RUN apt-get update -y && apt-get install -y libtool autoconf automake make g++ uuid-dev wget

#WORKDIR /tmp
#RUN wget http://download.zeromq.org/zeromq-4.0.4.tar.gz && tar -xvf zeromq-4.0.4.tar.gz && cd zeromq-* && ./configure && make install && ldconfig && cd .. && rm -rf zeromq*



# Setup
USER root
# Grab the zeromq 3xx tarball
WORKDIR /home/docker
RUN curl -L https://github.com/zeromq/zeromq4-1/releases/download/v4.1.6/zeromq-4.1.6.tar.gz > zeromq-4.1.6.tar.gz
RUN tar -xvf zeromq-4.1.6.tar.gz
# Install zeromq 3xx
WORKDIR /home/docker/zeromq-4.1.6
RUN ./configure
RUN make
RUN make install
RUN ldconfig

# Cleanup the tarball, the untarred directory and the headers
WORKDIR /home/docker
RUN rm -rf /home/docker/zeromq-4.1.6
RUN rm -rf /home/docker/zeromq-4.1.6.tar.gz
# Set library environment variable to allow linking
ENV LD_LIBRARY_PATH /usr/local/lib

#zbroker - the ZeroMQ broker project
#1. libsodium
WORKDIR /home/docker
RUN git clone git://github.com/jedisct1/libsodium.git
WORKDIR /home/docker/libsodium
RUN ./autogen.sh
RUN ./configure && make check
RUN make install
RUN ldconfig

#1. libzmq
WORKDIR /home/docker
RUN git clone git://github.com/zeromq/libzmq.git
WORKDIR /home/docker/libzmq
RUN ./autogen.sh
RUN ./configure && make check
RUN make install
RUN ldconfig

#2. czmq
WORKDIR /home/docker
RUN git clone git://github.com/zeromq/czmq.git
WORKDIR /home/docker/czmq
RUN ./autogen.sh
RUN ./configure && make check
RUN make install
RUN ldconfig

#3. zyre
WORKDIR /home/docker
RUN git clone git://github.com/zeromq/zyre.git
WORKDIR /home/docker/zyre
RUN ./autogen.sh
RUN ./configure && make check
RUN make install
RUN ldconfig

#4. zbroker
WORKDIR /home/docker
RUN git clone git://github.com/zeromq/zbroker.git
WORKDIR /home/docker/zbroker
RUN ./autogen.sh
RUN ./configure && make check
RUN make install
RUN ldconfig
##!!!!configure: error: cannot link with -lczmq, install libczmq.

RUN apk del \
    curl \
    g++ \
    make \
    automake \
    autoconf \
    libtool \
    util-linux-dev \
    git

USER docker
CMD /bin/bash