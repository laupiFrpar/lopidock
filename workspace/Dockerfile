ARG VERSION=7
FROM php:${VERSION}-cli

MAINTAINER Pierre-Louis Launay <laupi.frpar@gmail.com>

ENV DEBIAN_FRONTEND=noninteractive

# Add the "PHP 7" ppa, yarn package
# RUN apt-get install -y software-properties-common \
    # && add-apt-repository -y ppa:ondrej/php \
#     && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
#     && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        fontconfig \
        # libzip-dev \
        locales \
        locales-all \
        ncurses-term \
        unzip \
        zsh \
        zsh-antigen

ENV LANG="en_US.UTF-8" \
    LANGUAGE="en_US.UTF-8" \
    LC_ALL="en_US.UTF-8" \
    TERM=xterm-256color

COPY fonts /usr/share/fonts
COPY dotfiles/zshrc /root/.zshrc
COPY dotfiles/powerlevel9krc /root/.powerlevel9krc

#####################################
# PHP extensions
#####################################

# RUN docker-php-ext-install zip

    #     php-cli
#         php7.1-common \
#         php7.1-curl \
#         php7.1-intl \
#         php7.1-json \
#         php7.1-xml \
#         php7.1-mbstring \
#         php7.1-mcrypt \
#         php7.1-mysql \
#         php7.1-pgsql \
#         php7.1-sqlite \
#         php7.1-sqlite3 \
#         php7.1-zip \
#         php7.1-bcmath \
#         php7.1-memcached \
#         php7.1-gd \
#         php7.1-dev \
#         pkg-config \
#         libcurl4-openssl-dev \
#         libedit-dev \
#         libssl-dev \
#         libxml2-dev \
#         xz-utils \
#         libsqlite3-dev \
#         sqlite3 \
#         git \
#         curl \
#         vim \
#         nano

# #####################################
# # OPCACHE - APCU
# #####################################
# ARG PHP_INSTALL_OPCACHE=false
# ARG PHP_INSTALL_APCU=false
# ARG PHP_INSTALL_APCU_BC=false
# RUN if [ ${PHP_INSTALL_OPCACHE} = true ] && [ ${PHP_INSTALL_APCU} = false ]; then \
#     # Install opcache
#     echo "install opcache" \
# ;fi
# COPY ./usr/local/etc/php/conf.d/opcache.ini /usr/local/etc/php/conf.d/opcache.ini

# RUN if [ ${PHP_INSTALL_OPCACHE} = false ] && [ ${PHP_INSTALL_APCU} = true ]; then \
#     apt-get install -y --allow-downgrades --allow-remove-essential --allow-change-held-packages php-apcu \
# ;fi
# COPY ./usr/local/etc/php/conf.d/apcu.ini /usr/local/etc/php/conf.d/apcu.ini

# RUN if [ ${PHP_INSTALL_OPCACHE} = false ] && [ ${PHP_INSTALL_APCU} = true ] && [ ${PHP_INSTALL_APCU_BC} = true ]; then \
#     apt-get install -y --allow-downgrades --allow-remove-essential --allow-change-held-packages php-apcu-bc \
# ;fi

# #####################################
# # Composer:
# #####################################

COPY config/composer.json /root/.composer/composer.json

# Install composer and add its bin to the PATH.
RUN curl -s http://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer global install

# #####################################
# # Non-Root User:
# #####################################

# # Add a non-root user to prevent files being created with root permissions on host machine.
# ARG PUID=1000
# ARG PGID=1000

# ENV PUID ${PUID}
# ENV PGID ${PGID}

# RUN groupadd -g ${PGID} lopidock \
#     && useradd -u ${PUID} -g lopidock -m lopidock

# #####################################
# # Set Timezone
# #####################################

# ARG TZ=UTC
# ENV TZ ${TZ}
# RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# #####################################
# # Composer:
# #####################################

# # Add the composer.json
# COPY ./home/lopidock/composer/composer.json /home/lopidock/.composer/composer.json

# # Make sure that ~/.composer belongs to lopidock
# RUN chown -R lopidock:lopidock /home/lopidock/.composer
# USER lopidock

# # Check if global install need to be ran
# ARG COMPOSER_GLOBAL_INSTALL=false
# ENV COMPOSER_GLOBAL_INSTALL ${COMPOSER_GLOBAL_INSTALL}
# RUN if [ ${COMPOSER_GLOBAL_INSTALL} = true ]; then \
#     # run the install
#     composer global install \
# ;fi

# #####################################
# # User Aliases
# #####################################

# USER lopidock
# COPY ./home/lopidock/aliases.sh /home/lopidock/aliases.sh
# RUN echo "" >> ~/.bashrc \
#     && echo "# Load Custom Aliases" >> ~/.bashrc \
#     && echo "source /home/lopidock/aliases.sh" >> ~/.bashrc \
#     && echo "" >> ~/.bashrc \
#     && sed -i 's/\r//' /home/lopidock/aliases.sh \
#     && sed -i 's/^#! \/bin\/sh/#! \/bin\/bash/' /home/lopidock/aliases.sh

# USER root
# RUN echo "" >> ~/.bashrc \
#     && echo "# Load Custom Aliases" >> ~/.bashrc \
#     && echo "source /home/lopidock/aliases.sh" >> ~/.bashrc \
#     && echo "" >> ~/.bashrc \
#     && sed -i 's/\r//' /home/lopidock/aliases.sh \
#     && sed -i 's/^#! \/bin\/sh/#! \/bin\/bash/' /home/lopidock/aliases.sh

# #####################################
# # PHP REDIS EXTENSION FOR PHP 7.1
# #####################################

# RUN printf "\n" | pecl -q install -o -f redis \
#     && echo "extension=redis.so" >> /etc/php/7.1/mods-available/redis.ini \
#     && phpenmod redis

# #####################################
# # Non-root user : PHPUnit path
# #####################################

# # add ./vendor/bin to non-root user's bashrc (needed for phpunit)
# USER lopidock

# RUN echo "" >> ~/.bashrc \
#     && echo 'export PATH="/var/www/vendor/bin:$PATH"' >> ~/.bashrc

# #####################################
# # ImageMagick:
# #####################################
# USER root
# RUN apt-get install -y --force-yes imagemagick php-imagick

# #####################################
# # NVM
# #####################################
# ARG NODE_VERSION="node"
# RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash \
#   && . ~/.nvm/nvm.sh \
#   && nvm install ${NODE_VERSION} \
#   && nvm use ${NODE_VERSION}

# #####################################
# # Yarn
# #####################################
# RUN apt-get install --no-install-recommends yarn

# #
# #--------------------------------------------------------------------------
# # Final Touch
# #--------------------------------------------------------------------------
# #

# Clean up
# USER root
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && rm /var/log/lastlog /var/log/faillog

# Set default work directory
WORKDIR /var/www
