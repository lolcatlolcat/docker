# __    __  .______    _______   ______   ___   ___     _______   ______     ______  __  ___  _______ .______      
#|  |  |  | |   _  \  |       \ /  __  \  \  \ /  /    |       \ /  __  \   /      ||  |/  / |   ____||   _  \     
#|  |  |  | |  |_)  | |  .--.  |  |  |  |  \  V  /     |  .--.  |  |  |  | |  ,----'|  '  /  |  |__   |  |_)  |    
#|  |  |  | |   ___/  |  |  |  |  |  |  |   >   <      |  |  |  |  |  |  | |  |     |    <   |   __|  |      /     
#|  `--'  | |  |      |  '--'  |  `--'  |  /  .  \     |  '--'  |  `--'  | |  `----.|  .  \  |  |____ |  |\  \----.
# \______/  | _|      |_______/ \______/  /__/ \__\    |_______/ \______/   \______||__|\__\ |_______|| _| `._____|
#
# This is a docker container to install Snort++ (also known as Snort 3.0)
# Use the appropriate docker-compose file to launch this container.

FROM ubuntu:latest

#Install some Dependencies
RUN apt update -qq
ARG DEBIAN_FRONTEND=noninteractive
RUN apt install -y --no-install-recommends \
  cmake \
  make \
  gcc \
  g++ \
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
  gzip

# Make Directory to work out of
RUN mkdir /tmp/bro_tmp

# Setup GeoIP IPv4
WORKDIR /tmp/bro_tmp
RUN wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz \
  && gzip -d GeoLiteCity.dat.gz \
  && mv GeoLiteCity.dat /usr/share/GeoIP/
 
# Setup GeoIP IPv6
WORKDIR /tmp/bro_tmp
RUN wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCityv6-beta/GeoLiteCityv6.dat.gz \
  && gzip -d GeoLiteCityv6.dat.gz \
  && mv GeoLiteCityv6.dat /usr/share/GeoIP/GeoIPCityv6.dat

# Install BRO
WORKDIR /tmp/bro_tmp
RUN git clone --recursive git://git.bro.org/bro \
  && ./configure --prefix=/usr/local \
  && make -j 8 \
  && make install -j 8

# Run Bro
CMD broctl deploy
