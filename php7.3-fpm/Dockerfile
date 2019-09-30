FROM php:7.3-fpm
ARG TIMEZONE

# php ini set memory limit
COPY "memory-limit-php.ini" "/usr/local/etc/php/conf.d/memory-limit-php.ini"

RUN rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install -y sudo \
    openssl \
    git \
    unzip \
    gnupg2 \
    apt-utils \
    zlib1g-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libicu-dev \
    libxml2-dev \
    libmcrypt-dev \
    libcurl4-openssl-dev \
    pkg-config \
    libssl-dev \
    bash \
    g++ \
    libzip-dev \
    iputils-ping \
    vim

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini
RUN "date"

# configure intl
RUN docker-php-ext-configure intl

# Type docker-php-ext-install to see available extensions
RUN docker-php-ext-install pdo pdo_mysql zip gd mbstring intl soap

# install apcu
RUN pecl install apcu \
    && docker-php-ext-enable apcu

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
&& docker-php-ext-install -j$(nproc) gd

RUN pecl install mongodb \
&& docker-php-ext-enable mongodb

RUN apt-get update || apt-get update \
    && apt-get install -y \
        librabbitmq-dev \
        libssh-dev \
    && docker-php-ext-install \
        bcmath \
        sockets \
    && pecl install amqp \
    && docker-php-ext-enable amqp

RUN chmod -R a+rw /var/www/

RUN adduser --disabled-password --home /home/user user
RUN addgroup user users

# Cleanup
RUN apt-get update && apt-get upgrade -y && apt-get autoremove -y

USER user

WORKDIR /var/www
