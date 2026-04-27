FROM php:8.3-fpm-alpine

WORKDIR /var/www

# Install dependencies
RUN apk update && apk add \
    build-base \
    freetype-dev \
    libjpeg-turbo-dev \
    libpng-dev \
    libzip-dev \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl \
    oniguruma-dev


RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl

RUN docker-php-ext-configure gd \
    --with-freetype \
    --with-jpeg

RUN docker-php-ext-install gd


# (optionnel mais utile)
RUN echo "clear_env = no" >> /usr/local/etc/php-fpm.d/www.conf

# Copy config
COPY ./config/php/local.ini /usr/local/etc/php/conf.d/local.ini

RUN addgroup -g 1000 -S www && \
    adduser -u 1000 -S www -G www

RUN chown -R www:www /var/www

USER www

COPY --chown=www:www . /var/www

EXPOSE 9000