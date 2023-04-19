# Set our base image
FROM serversideup/php:8.1-fpm-nginx

LABEL maintainer="Qisthi Ramadhani"

COPY .bash_aliases /root/.bash_aliases
COPY oracle /opt/oracle
COPY 10-oci8.ini /etc/php/8.1/cli/conf.d/10-oci8.ini
COPY 10-oci8.ini /etc/php/8.1/fpm/conf.d/10-oci8.ini

# Additional
RUN apt-get update \
    && apt-get install -y --no-install-recommends libaio1 git \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*
ENV LD_LIBRARY_PATH="/opt/oracle/instantclient_19_10"

# NVM
RUN mkdir /usr/local/nvm
ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION 18.16.0
RUN curl https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash \
    && . $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default \
    && corepack enable
ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH
