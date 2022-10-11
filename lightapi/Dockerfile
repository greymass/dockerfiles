# runtime image
FROM ubuntu:20.04

# setup build time variables
ARG LIGHTAPI_REPOSITORY=https://github.com/cc32d9/eosio_light_api.git

# deps
RUN apt-get update \
    && DEBIAN_FRONTEND="noninteractive" apt-get install -y --no-install-recommends \
        git \
        ca-certificates \
        curl \
        make \
        cpanminus \
        gcc \
        g++ \
        libmysqlclient-dev \
        libdbi-perl \
        libjson-xs-perl \
        libjson-perl \
        libdatetime-format-iso8601-perl \
    && cpanm --notest DBD::MariaDB \
    && cpanm --notest Starman \
    && cpanm --notest Net::WebSocket::Server \
    && cpanm --notest Crypt::Digest::RIPEMD160 \
    && curl -sL https://deb.nodesource.com/setup_13.x | bash - \
    && apt install -y nodejs \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /opt/eosio_light_api
RUN git clone --branch master --depth 1 $LIGHTAPI_REPOSITORY .
WORKDIR /opt/eosio_light_api/wsapi
RUN npm install
WORKDIR /opt/eosio_light_api

COPY add_basic_tables.sql add_chain_specific_tables.sql /opt/eosio_light_api/greymass_addons/
