ARG VERSION=latest
FROM mysql:${VERSION}

MAINTAINER Pierre-Louis Launay <laupi.frpar@gmail.com>

#################
# Set Timezone
#################

ARG TZ=UTC
ENV TZ ${TZ}
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN chown -R mysql:root /var/lib/mysql/

ADD ./etc/mysql/conf.d/my.cnf /etc/mysql/conf.d/my.cnf

CMD ["mysqld"]

EXPOSE 3306
