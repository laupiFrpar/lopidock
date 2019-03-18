ARG VERSION=7
FROM php:${VERSION}-cli

LABEL maintainer="Pierre-Louis Launay <laupi.frpar@gmail.com>"

ENV DEBIAN_FRONTEND=noninteractive

# Add the "PHP 7" ppa, yarn package
# RUN apt-get install -y software-properties-common \
    # && add-apt-repository -y ppa:ondrej/php \
#     && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
#     && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        fontconfig \
        locales \
        locales-all \
        ncurses-term \
        unzip \
        vim \
        zsh \
        zsh-antigen

ENV LANG="en_US.UTF-8" \
    LANGUAGE="en_US.UTF-8" \
    LC_ALL="en_US.UTF-8" \
    TERM=xterm-256color \
    TZ=UTC

COPY fonts /usr/share/fonts
COPY dotfiles/zshrc /root/.zshrc
COPY dotfiles/powerlevel9krc /root/.powerlevel9krc

# #####################################
# # Composer:
# #####################################

COPY config/composer.json /root/.composer/composer.json

# Install composer and add its bin to the PATH.
RUN curl -s http://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer global install

# # Make sure that ~/.composer belongs to lopidock
# RUN chown -R lopidock:lopidock /home/lopidock/.composer
# USER lopidock

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

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

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
# # Non-root user : PHPUnit path
# #####################################

# # add ./vendor/bin to non-root user's bashrc (needed for phpunit)
# USER lopidock

# RUN echo "" >> ~/.bashrc \
#     && echo 'export PATH="/var/www/vendor/bin:$PATH"' >> ~/.bashrc

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
