# This is a docker container to install Snort++ (also known as Snort 3.0)
# Use the appropriate docker-compose file to launch this container.

FROM debian:latest

#Install some Dependencies
RUN apt update -qq
ARG DEBIAN_FRONTEND=noninteractive
RUN apt install -y \
  net-tools \
  build-essential \
  autotools-dev \
  libdumbnet-dev \
  libluajit-5.1-dev \
  libpcap-dev \
  libpcre3-dev \
  zlib1g-dev \
  pkg-config \
  libhwloc-dev \
  cmake \
  liblzma-dev \
  openssl \
  libssl-dev \
  cpputest \
  libsqlite3-dev \
  uuid-dev \
  asciidoc \
  dblatex \
  source-highlight \
  w3m \
  libtool \
  git \
  autoconf \
  bison \
  flex \
  libnetfilter-queue-dev \
  wget

# Make Temporary DIR
RUN mkdir /tmp/snort_src

# Get Safec
WORKDIR /tmp/snort_src
RUN wget https://downloads.sourceforge.net/project/safeclib/libsafec-10052013.tar.gz && \
  tar -xzvf libsafec-10052013.tar.gz
WORKDIR /tmp/snort_src/libsafec-10052013
RUN ./configure && make -j 8 && make install -j 8

# Get Gperftools
WORKDIR /tmp/snort_src
RUN wget https://github.com/gperftools/gperftools/releases/download/gperftools-2.7/gperftools-2.7.tar.gz && \
  tar xzvf gperftools-2.7.tar.gz
WORKDIR /tmp/snort_src/gperftools-2.7
RUN ./configure && make -j 8 && make install -j 8

# Get Ragel
WORKDIR /tmp/snort_src
RUN wget http://www.colm.net/files/ragel/ragel-6.10.tar.gz && \
  tar xzvf ragel-6.10.tar.gz
WORKDIR /tmp/snort_src/ragel-6.10
RUN ./configure && make -j 8 && make install -j 8

# Get Boost
WORKDIR /tmp/snort_src
RUN wget https://dl.bintray.com/boostorg/release/1.67.0/source/boost_1_67_0.tar.gz && \
  tar xzvf boost_1_67_0.tar.gz

# Get Hyperscan
WORKDIR /tmp/snort_src
RUN wget https://github.com/intel/hyperscan/archive/v4.7.0.tar.gz && \
  tar xzvf v4.7.0.tar.gz && \
  mkdir hyperscan-4.7.0-build
WORKDIR /tmp/snort_src/hyperscan-4.7.0-build
RUN cmake -DCMAKE_INSTALL_PREFIX=/usr/local -DBOOST_ROOT=/tmp/snort_src/boost_1_67_0/ ../hyperscan-4.7.0 && \
  make -j 8 && \
  make install -j 8

# Get flatbuffers
WORKDIR /tmp/snort_src
RUN wget https://github.com/google/flatbuffers/archive/v1.9.0.tar.gz -O flatbuffers-v1.9.0.tar.gz && \
  tar xzvf flatbuffers-v1.9.0.tar.gz && \
  mkdir flatbuffers-build
WORKDIR /tmp/snort_src/flatbuffers-build
RUN cmake ../flatbuffers-1.9.0 && \
  make -j 8 && \
  make install -j 8

# Get Snort DAQ
WORKDIR /tmp/snort_src
RUN wget https://www.snort.org/downloads/snortplus/daq-2.2.2.tar.gz && \
  tar xzvf daq-2.2.2.tar.gz
WORKDIR /tmp/snort_src/daq-2.2.2
RUN ./configure && make -j 8 && make install -j 8
RUN ldconfig

# Get Snort 3.0
WORKDIR /tmp/snort_src/
RUN git clone git://github.com/snortadmin/snort3.git
WORKDIR /tmp/snort_src/snort3
RUN ./configure_cmake.sh --prefix=/usr/local --enable-tcmalloc --enable-shell && \
  cd build && \
  make -j 8 && \
  make install -j 8

# Need some ENV variables for things to work
ENV LUA_PATH /usr/local/include/snort/lua/\?.lua\;\;
ENV SNORT_LUA_PATH /usr/local/etc/snort

# Get Snort Started
ENTRYPOINT snort -i eth0 -c /usr/local/etc/snort/snort.lua -l /var/log/snort
