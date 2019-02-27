FROM nginx:alpine

MAINTAINER Pierre-Louis Launay <laupi.frpar@gmail.com>

RUN apk update \
  && apk upgrade \
  && apk add --no-cache bash \
  && adduser -D -H -u 1000 -s /bin/bash www-data

# Set upstream conf and remove the default conf
ARG PHP_UPSTREAM_CONTAINER=php-fpm
ARG PHP_UPSTREAM_PORT=9000
RUN echo "upstream php-upstream { server ${PHP_UPSTREAM_CONTAINER}:${PHP_UPSTREAM_PORT}; }" > /etc/nginx/conf.d/upstream.conf \
  && rm /etc/nginx/conf.d/default.conf

COPY ./etc/nginx/nginx.conf /etc/nginx/

CMD ["nginx"]

EXPOSE 80 443
