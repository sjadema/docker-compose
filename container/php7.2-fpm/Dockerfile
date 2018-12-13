FROM php:7.2-fpm

MAINTAINER Sjoerd Adema <sjoerd@triz.nu>

# We don't have a frontend
ENV DEBIAN_FRONTEND=noninteractive

# Download dependencies
RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg-dev \
    libpng-dev \
    libicu-dev \
    zlib1g-dev \
    libpq-dev \
    unzip \
    zip

# Build & enable PHP modules
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install -j$(nproc) \
    opcache \
    intl \
    pdo_pgsql
#    gd

# Enable PECL modules
RUN pecl install igbinary && docker-php-ext-enable igbinary
RUN pecl install redis && docker-php-ext-enable redis
RUN pecl install xdebug

# Copy correct ini settings
COPY resources/usr/local/etc/php/php.ini $PHP_INI_DIR/php.ini

# Make sure the correct IDs map on the user
RUN groupmod -g 1000 www-data
RUN usermod  -u 1000 www-data
