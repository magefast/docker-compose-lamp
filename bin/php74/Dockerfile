FROM php:7.4-apache

#Surpresses debconf complaints of trying to install apt packages interactively
#https://github.com/moby/moby/issues/4032#issuecomment-192327844

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get -y update --fix-missing --no-install-recommends
RUN apt-get -y upgrade

# Install useful tools
RUN apt-get -yq install apt-utils nano wget dialog

# Install important libraries
RUN apt-get -y install --fix-missing apt-utils build-essential git curl openssl

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install redis
RUN pecl install redis-4.0.1
RUN docker-php-ext-enable redis

# Other PHP7 Extensions

RUN apt-get -y install libsqlite3-dev libsqlite3-0
RUN docker-php-ext-install pdo_mysql 
RUN docker-php-ext-install pdo_sqlite
RUN docker-php-ext-install mysqli


RUN docker-php-ext-install tokenizer
RUN docker-php-ext-install json

RUN apt-get -y install zlib1g-dev
RUN apt-get -y install libicu-dev
RUN docker-php-ext-install -j$(nproc) intl

RUN docker-php-ext-install gettext

# XML
RUN apt-get update -y \
  && apt-get install -y \
     libxml2-dev \
  && apt-get clean -y \
  && docker-php-ext-install soap

RUN apt-get install -y libzip-dev zip \
    && docker-php-ext-install zip

# BCMATH
RUN docker-php-ext-install bcmath
RUN docker-php-ext-enable bcmath

RUN docker-php-ext-install sockets
RUN docker-php-ext-enable sockets

# XSL
RUN apt install -y libxslt-dev
RUN docker-php-ext-install xsl

RUN apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install -j$(nproc) gd


RUN apt-get install -y ssl-cert

#RUN apt-get install libsodium-dev -y
#RUN docker-php-ext-install sodium

RUN apt install -y libsodium-dev
RUN docker-php-ext-configure sodium
RUN docker-php-ext-install sodium
#RUN pecl install libsodium-2.0.21
RUN docker-php-ext-enable sodium


#RUN apt-get update \
# && DEBIAN_FRONTEND=noninteractive apt-get install -y ssl-cert \
# && rm -r /var/lib/apt/lists/* \

# Enable apache modules
RUN a2enmod rewrite headers expires ssl
RUN a2ensite default-ssl

# IMAGICK
RUN apt-get update && apt-get install -y libmagickwand-dev --no-install-recommends && rm -rf /var/lib/apt/lists/*
RUN printf "\n" | pecl install imagick
RUN docker-php-ext-enable imagick

# SSL
RUN mkdir /etc/apache2/ssl
RUN openssl req -new -x509 -nodes -out /etc/apache2/ssl/server.pem -keyout /etc/apache2/ssl/server.key -days 3650 -subj '/CN=xxx.localhost'


# Setup Apache2 mod_ssl
RUN a2enmod ssl

# Setup Apache2 HTTPS env
RUN a2ensite default-ssl.conf