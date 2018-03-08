FROM nginx:1.13

ARG GRAV_VERSION=1.3.10

RUN apt-get update
RUN apt-get install -y  php php-fpm php-curl php-ctype php-dom php-gd php-json php-mbstring php-simplexml php-xml php-zip
RUN apt-get install -y wget unzip git

RUN mkdir -p /var/www && chown www-data:www-data /var/www
USER www-data
WORKDIR /var/www

RUN wget https://github.com/getgrav/grav/releases/download/$GRAV_VERSION/grav-admin-v$GRAV_VERSION.zip && \
    unzip grav-admin-v$GRAV_VERSION.zip && \
    rm grav-admin-v$GRAV_VERSION.zip && \
    cd grav-admin && \
bin/gpm install -f -y admin

USER root
RUN usermod -aG www-data nginx

RUN rm /etc/nginx/conf.d/default.conf
COPY conf/* /etc/nginx/conf.d/

VOLUME /var/www/grav-admin
CMD service php7.0-fpm start && service nginx start && tail -f /var/log/nginx/error.log
