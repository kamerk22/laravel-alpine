FROM php:8.4-fpm-alpine

# Add Build Dependencies
RUN apk update && apk add --no-cache --virtual .build-deps  \
    zlib-dev \
    libjpeg-turbo-dev \
    libpng-dev \
    libxml2-dev \
    bzip2-dev \
    zip 

# Add Production Dependencies
RUN apk add --update --no-cache \
    jpegoptim \
    pngquant \
    optipng \
    supervisor \
    nano \
    icu-dev \
    freetype-dev \
    mysql-client \ 
    libzip-dev

RUN php -m

# # Configure & Install Extension
RUN docker-php-ext-configure gd --with-freetype --with-jpeg; \
    docker-php-ext-install -j"$(nproc)" \
    pdo_mysql \
    bcmath \
    pcntl \
    zip \
    gd

# Add Composer
RUN curl -s https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer
ENV COMPOSER_ALLOW_SUPERUSER=1
ENV PATH="./vendor/bin:$PATH"

# Configure PHP for FPM
COPY php.ini $PHP_INI_DIR/conf.d/

# Create directories
RUN mkdir -p /var/www/ /var/log/php

# Remove Build Dependencies
RUN apk del -f .build-deps 

# Setup Working Dir
WORKDIR /var/www
