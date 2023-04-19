# Set our base image
FROM serversideup/php:8.1-fpm-nginx

LABEL maintainer="Qisthi Ramadhani"

COPY .bash_aliases /root/.bash_aliases

# Install prerequisites
RUN apt-get update \
    && apt-get install -y php8.1-dev php-pear build-essential libaio-dev libaio1 git nano wget zip unzip libc6-amd64-cross \
    && pecl channel-update pecl.php.net

# Setup Oracle Instant Client
RUN mkdir /opt/oracle \
    && cd /opt/oracle \
    && wget https://download.oracle.com/otn_software/linux/instantclient/191000/instantclient-basic-linux.arm64-19.10.0.0.0dbru-2.zip \
    && wget https://download.oracle.com/otn_software/linux/instantclient/191000/instantclient-sdk-linux.arm64-19.10.0.0.0dbru.zip \
    && wget https://download.oracle.com/otn_software/linux/instantclient/191000/instantclient-sqlplus-linux.arm64-19.10.0.0.0dbru.zip \
    && unzip instantclient-basic-linux.arm64-19.10.0.0.0dbru-2.zip \
    && unzip instantclient-sdk-linux.arm64-19.10.0.0.0dbru.zip \
    && unzip instantclient-sqlplus-linux.arm64-19.10.0.0.0dbru.zip \
    && sh -c "echo /opt/oracle/instantclient_19_10 > /etc/ld.so.conf.d/oracle-instantclient.conf" \
    && export LD_LIBRARY_PATH="/opt/oracle/instantclient_19_10"

# Install OCI8 extension
RUN cd /opt/oracle \
    && wget https://pecl.php.net/get/oci8-3.2.1.tgz \
    && tar xzf oci8-3.2.1.tgz \
    && cd oci8-3.2.1 \
    && phpize8.1 \
    && ./configure --with-oci8=instantclient,/opt/oracle/instantclient_19_10 --with-php-config=/usr/bin/php-config8.1 \
    && make \
    && touch /etc/php/8.1/cli/conf.d/10-oci8.ini \
    && sh -c "echo 'extension=/opt/oracle/oci8-3.2.1/modules/oci8.so' > /etc/php/8.1/cli/conf.d/10-oci8.ini" \
    && touch /etc/php/8.1/fpm/conf.d/10-oci8.ini \
    && sh -c "echo 'extension=/opt/oracle/oci8-3.2.1/modules/oci8.so' > /etc/php/8.1/fpm/conf.d/10-oci8.ini"

# Cleanup
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* /opt/oracle/*.zip /opt/oracle/*.tgz /opt/oracle/package.xml

ENV LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/opt/oracle/instantclient_19_10"
