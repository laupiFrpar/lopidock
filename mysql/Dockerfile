ARG VERSION=latest
FROM mysql:${VERSION}

LABEL maintainer="Pierre-Louis Launay <laupi.frpar@gmail.com>"

#################
# Set Timezone
#################

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN chown -R mysql:root /var/lib/mysql/
