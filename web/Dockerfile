#
# DOCKER-VERSION 1.0.1
#
FROM debian:wheezy

# tag rds13/debian-serf-web
MAINTAINER rds13 "https://github.com/rds13"

# Install serf
RUN apt-get update
RUN apt-get install -y unzip wget
RUN wget --no-check-certificate https://dl.bintray.com/mitchellh/serf/0.5.0_linux_amd64.zip -O serf.zip
RUN unzip serf.zip && rm serf.zip
RUN chmod +x serf && mv serf /usr/bin/serf

# Install nginx
RUN apt-get install -qy nginx

# Install supervisor
RUN apt-get install -qy supervisor

# Supervisord config
ADD supervisord-nginx.conf /etc/supervisor/conf.d/supervisord-nginx.conf

# Default page
ADD index.html /usr/share/nginx/www/index.html

# Startup script (We need this to use environmental variables with --link)
ADD start-serf.sh /start-serf.sh
RUN chmod 755 /start-serf.sh

EXPOSE 80
CMD ["supervisord", "-n"]
