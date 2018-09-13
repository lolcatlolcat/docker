#Dockerfile to install Bro

FROM debian:stretch

#Install some Dependencies
RUN apt update -qq
ARG DEBIAN_FRONTEND=noninteractive
RUN apt install -y \
  cmake \
  make \
  gcc \
  g++ \
  build-essential \
  flex \
  git \
  bison \
  libpcap-dev \
  libssl-dev \
  python-dev \
  swig \
  zlib1g-dev \
  libgeoip-dev \
  wget \
  net-tools \
  gzip \
  supervisor

# Make Directory to work out of
RUN mkdir /tmp/bro_tmp

# Install BRO
WORKDIR /tmp/bro_tmp
RUN git clone --recursive git://git.bro.org/bro
WORKDIR /tmp/bro_tmp/bro
RUN ./configure --prefix=/opt/bro \
  && make -j 8 \
  && make install -j 8

# Configure some ENV
ENV PATH $PATH:/opt/bro/bin:$PATH

#Cleanup 
RUN apt clean && apt autoremove -y
RUN rm -rf /tmp/bro_tmp

#Add files
COPY bro.conf /etc/supervisor/supervisord.conf
COPY configs/local.bro /opt/bro/share/bro/site/local.bro
COPY configs/node.cfg /opt/bro/etc/node.cfg

#Run Bro, Bro
WORKDIR /opt/bro
ENTRYPOINT ["supervisord"]
