FROM node:12.18.1-slim AS node
FROM php:7.1-apache

COPY --from=node /usr/local/bin /usr/local/bin
COPY --from=node /usr/lib /usr/lib
COPY --from=node /opt /opt
COPY --from=composer:1.10.17 /usr/bin/composer /usr/bin/composer

RUN apt-get update

RUN apt-get install -y --no-install-recommends \
        wget curl zip git zsh vim \
        apt-utils \
        sendmail-bin \
        sendmail \
        libbz2-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libfreetype6-dev \
        libgeoip-dev \
        libgmp-dev \
        libmagickwand-dev \
        libmagickcore-dev \
        libc-client-dev \
        libkrb5-dev \
        libicu-dev \
        libldap2-dev \
        libpspell-dev \
        librecode0 \
        librecode-dev \
        libssh2-1 \
        libssh2-1-dev \
        libtidy-dev \
        libxslt1-dev \
        libyaml-dev \
        libzip-dev \
        mariadb-client \
        libonig-dev \
        libmcrypt-dev \
        libxslt-dev \
        autoconf automake make gcc g++ icu-devtools

RUN pecl install xdebug-2.6.0

RUN pecl install apcu

RUN docker-php-ext-enable xdebug

RUN docker-php-ext-configure gd \
    --with-freetype-dir=/usr/include --with-jpeg-dir=/usr/include

RUN docker-php-ext-configure \
    imap --with-kerberos --with-imap-ssl

RUN docker-php-ext-configure \
    opcache --enable-opcache

RUN docker-php-ext-configure zip

RUN docker-php-ext-configure intl

RUN docker-php-ext-install -j$(nproc) \
    bcmath \
    bz2 \
    calendar \
    exif \
    gettext \
    gmp \
    imap \
    intl \
    opcache \
    shmop \
    soap \
    sockets \
    sysvmsg \
    sysvsem \
    sysvshm \
    tidy \
    zip \
    mysqli \
    gd \
    mbstring \
    pdo_mysql \
    mcrypt \
    xsl \
    xmlrpc \
    opcache

RUN docker-php-ext-enable apcu opcache

COPY etc/infrastructure/php/ /usr/local/etc/php/

RUN usermod -u 1000 www-data
RUN chsh -s /bin/zsh www-data

RUN set -uex

RUN wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh
RUN sh ./install.sh
RUN rm ./install.sh

RUN a2enmod rewrite
RUN a2enmod headers

ENV XDEBUG_PORT 9000
