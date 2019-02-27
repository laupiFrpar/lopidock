# Inpsired from https://github.com/schickling/dockerfiles/blob/master/mailcatcher/Dockerfile
ARG VERSION=0.6.5

FROM ruby:2.4-alpine3.6

MAINTAINER Pierre-Louis Launay <laupi.frpar@gmail.com>

RUN set -xe \
    && apk add --no-cache \
        libstdc++ \
        sqlite-libs \
    && apk add --no-cache --virtual .build-deps \
        build-base \
        sqlite-dev \
    && gem install mailcatcher -v ${VERSION} --no-ri --no-rdoc \
    && apk del .build-deps

CMD ["mailcatcher", "--no-quit", "--foreground", "--ip=0.0.0.0"]

EXPOSE 1025 1080
