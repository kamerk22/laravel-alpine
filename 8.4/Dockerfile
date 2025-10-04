# Stage 1: Build environment and Composer dependencies
FROM php:8.4-fpm-alpine3.22

# Install system dependencies and PHP extensions required for Laravel + MySQL support
# Some dependencies are required for PHP extensions only in the build stage
# We don't need to install Node.js or build assets, as it was done in the Nginx image
RUN apk add --no-cache \
    curl \
    nodejs \
    npm \
    unzip \
    imagemagick \
    imagemagick-dev \
    oniguruma-dev \
    openssl-dev \
    libxml2-dev \
    curl-dev \
    icu-dev \
    libzip-dev \
    $PHPIZE_DEPS \
    && docker-php-ext-install -j$(nproc) \
    pdo_mysql \
    pcntl \
    opcache \
    intl \
    zip \
    bcmath \
    soap \
    exif \
    && CPPFLAGS='-Dphp_strtolower=zend_str_tolower' pecl install redis imagick \
    && docker-php-ext-enable redis imagick \
    && apk del $PHPIZE_DEPS \
    && rm -rf /tmp/* /var/tmp/*

# Install Composer and dependencies
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer 