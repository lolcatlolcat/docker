# This is a docker container to install Snort++ (also known as Snort 3.0)
# Use the appropriate docker-compose file to launch this container.

FROM debian:latest

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
  gzip

# Make Directory to work out of
RUN mkdir /tmp/bro_tmp

# Setup GeoIP IPv4
WORKDIR /tmp/bro_tmp
RUN wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz \
  && gzip -d GeoLiteCity.dat.gz \
  && mv GeoLiteCity.dat /usr/share/GeoIP/GeoIPCity.dat
 
# Setup GeoIP IPv6
WORKDIR /tmp/bro_tmp
RUN wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCityv6-beta/GeoLiteCityv6.dat.gz \
  && gzip -d GeoLiteCityv6.dat.gz \
  && mv GeoLiteCityv6.dat /usr/share/GeoIP/GeoIPCityv6.at

# Install BRO
WORKDIR /tmp/bro_tmp
RUN git clone --recursive git://git.bro.org/bro
WORKDIR /tmp/bro_tmp/bro
RUN ./configure --prefix=/opt/bro --with-geoip=/usr/share/GeoIP/  \
  && make -j 8 \
  && make install -j 8

# Configure some ENV
ENV PATH $PATH:/opt/bro/bin:$PATH

# Run Bro
RUN broctl deploy
WORKDIR /opt/bro
ENTRYPOINT ["/bin/sh"]
