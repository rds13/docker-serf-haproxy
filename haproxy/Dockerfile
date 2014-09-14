#
# DOCKER-VERSION 1.0.1
#
FROM rds13/debian-rbenv

# tag rds13/debian-serf-haproxy
MAINTAINER rds13 "https://github.com/rds13"

#
# Install serf
RUN apt-get update
RUN apt-get install -y unzip wget
RUN wget --no-check-certificate https://dl.bintray.com/mitchellh/serf/0.5.0_linux_amd64.zip -O serf.zip
RUN unzip serf.zip && rm serf.zip
RUN chmod +x serf && mv serf /usr/bin/serf

# Install supervisor
RUN apt-get install -qy supervisor

# Install HAProxy
#WORKDIR /root
#RUN wget http://www.haproxy.org/download/1.5/src/haproxy-1.5.1.tar.gz
#RUN tar zxvf haproxy-1.5.1.tar.gz
#RUN cd haproxy-1.5.1 && TARGET=linux2628 USE_STATIC_PCRE=1 USE_OPENSSL=1 && make install

RUN wget -O - http://haproxy.debian.net/bernat.debian.org.gpg | apt-key add -
RUN echo "deb http://cdn.debian.net/debian wheezy-backports main" > /etc/apt/sources.list.d/backports.list
RUN echo "deb http://haproxy.debian.net/ wheezy-backports main" > /etc/apt/sources.list.d/haproxy.list
RUN apt-get update
RUN apt-get install -t wheezy-backports -y haproxy

# Add Handler
ADD default_haproxy.cfg /etc/haproxy/haproxy.cfg
ADD supervisord-haproxy.conf /etc/supervisor/conf.d/supervisord-haproxy.conf
ADD handler.rb /handler.rb

EXPOSE 80
CMD ["bash", "-l", "-c", "supervisord -n"]
