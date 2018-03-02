#
#--------------------------------------------------------------------------
# Image Setup
#--------------------------------------------------------------------------
#

FROM php:7.1-fpm

MAINTAINER Pierre-Louis Launay <laupi.frpar@gmail.com>

RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    curl \
    libmemcached-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng12-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
    libmagickwand-dev \
    imagemagick \
  && rm -rf /var/lib/apt/lists/*

# Install the PHP mcrypt extention
RUN docker-php-ext-install mcrypt pdo_mysql zip bcmath opcache \
  && docker-php-ext-configure gd \
    --enable-gd-native-ttf \
    --with-jpeg-dir=/usr/lib \
    --with-freetype-dir=/usr/include/freetype2 \
  && docker-php-ext-install gd

# Copy opcache configration
COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini

#####################################
# PHP REDIS EXTENSION FOR PHP 7.0
#####################################

RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis

#####################################
# Human Language and Character Encoding Support:
#####################################

ARG INSTALL_ICU_VERSION=60.1
ENV PHP_CPPFLAGS="$PHP_CPPFLAGS -std=c++11"
RUN curl -fsS -o /tmp/icu.tgz -L http://download.icu-project.org/files/icu4c/${INSTALL_ICU_VERSION}/icu4c-$(echo $INSTALL_ICU_VERSION | sed 's/\./_/')-src.tgz \
	&& tar -zxf /tmp/icu.tgz -C /tmp \
	&& cd /tmp/icu/source \
	&& ./configure --prefix=/usr/local \
	&& make \
	&& make install \
	&& rm -rf /tmp/icu* \
	&& docker-php-ext-configure intl --with-icu-dir=/usr/local \
	&& docker-php-ext-install intl

#####################################
# ImageMagick:
#####################################
USER root
RUN pecl install imagick \
	&& docker-php-ext-enable imagick

ADD ./custom.ini /usr/local/etc/php/conf.d

RUN usermod -u 1000 www-data

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000
