# vim:set ft=dockerfile:
FROM evaneos/postgres:9.3-alpine

ENV POSTGIS_VERSION 2.3.1

RUN echo http://dl-cdn.alpinelinux.org/alpine/edge/main >> /etc/apk/repositories
RUN echo http://dl-cdn.alpinelinux.org/alpine/edge/testing >> /etc/apk/repositories

RUN apk add --no-cache --virtual .build-deps curl g++ make autoconf libxml2-dev geos-dev gdal-dev proj4-dev perl
RUN apk add --no-cache --virtual .run-deps geos gdal proj4
RUN curl -L -o postgis.tar.gz http://download.osgeo.org/postgis/source/postgis-$POSTGIS_VERSION.tar.gz
RUN mkdir -p /usr/src/postgis
RUN tar \
        --extract \
        --file postgis.tar.gz \
        --directory /usr/src/postgis \
        --strip-components 1
RUN rm postgis.tar.gz
WORKDIR /usr/src/postgis
RUN ./configure
RUN make
RUN make install
RUN rm -rf /usr/src/postgis
RUN apk del .build-deps

WORKDIR /

COPY ./initdb-postgis.sh /docker-entrypoint-initdb.d/postgis.sh