FROM php:7.2-fpm

# We don't have a frontend
ENV DEBIAN_FRONTEND=noninteractive

# Download dependencies
RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg-dev \
    libpng-dev \
    libicu-dev \
    zlib1g-dev

# Build & enable PHP modules
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install -j$(nproc) \
    opcache \
    intl
#    gd

# Enable PECL modules
RUN pecl install igbinary redis  \
    && docker-php-ext-enable \
    igbinary \
    redis

# Copy correct ini settings
COPY resources/usr/local/etc/php/php.ini $PHP_INI_DIR/php.ini
