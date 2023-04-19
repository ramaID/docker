# Set our base image
FROM serversideup/php:8.1-fpm-nginx

LABEL maintainer="Qisthi Ramadhani"

COPY .bash_aliases /root/.bash_aliases
COPY oracle /opt/oracle
COPY 10-oci8.ini /etc/php/8.1/cli/conf.d/10-oci8.ini
COPY 10-oci8.ini /etc/php/8.1/fpm/conf.d/10-oci8.ini

# Additional
RUN apt-get update \
    && apt-get install -y --no-install-recommends libaio1 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*
ENV LD_LIBRARY_PATH="/opt/oracle/instantclient_19_10"
