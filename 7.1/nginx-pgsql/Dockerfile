FROM php:7.1-fpm-alpine3.8

LABEL Maintainer Kashyap Merai <kashyapk62@gmail.com>

# Add Repositories
RUN rm -f /etc/apk/repositories &&\ 
    echo "http://dl-cdn.alpinelinux.org/alpine/v3.8/main" >> /etc/apk/repositories && \
    echo "http://dl-cdn.alpinelinux.org/alpine/v3.8/community" >> /etc/apk/repositories

# Add Build Dependencies
RUN apk add --no-cache --virtual .build-deps  \
    zlib-dev \
    libjpeg-turbo-dev \
    libpng-dev \
    libxml2-dev \
    bzip2-dev \
    postgresql-dev

# Add Production Dependencies
RUN apk add --update --no-cache \
    jpegoptim \ 
    pngquant \ 
    optipng \
    supervisor \
    nano \
    icu-dev \
    freetype-dev \
    nginx \ 
    mysql-client

# Configure & Install Extension
RUN docker-php-ext-configure \
    opcache --enable-opcache &&\
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ &&\ 
    docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql &&\
    docker-php-ext-install \ 
    opcache \ 
    pdo_pgsql \
    pgsql \
    pdo \ 
    sockets \ 
    json \
    intl \
    gd \
    xml \
    zip \
    bz2 \
    pcntl \
    bcmath

# Add Composer
RUN curl -s https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer 
ENV COMPOSER_ALLOW_SUPERUSER=1
ENV PATH="./vendor/bin:$PATH"

COPY opcache.ini $PHP_INI_DIR/conf.d/
COPY php.ini $PHP_INI_DIR/conf.d/

# Setup Crond and Supervisor by default
RUN echo '*  *  *  *  * /usr/local/bin/php  /var/www/artisan schedule:run >> /dev/null 2>&1' > /etc/crontabs/root && mkdir /etc/supervisor.d 
ADD master.ini /etc/supervisor.d/
ADD default.conf /etc/nginx/conf.d/ 

# Remove Build Dependencies
RUN apk del -f .build-deps
# Setup Working Dir
WORKDIR /var/www

CMD ["/usr/bin/supervisord"]
