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

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl

# Configure GD properly
RUN docker-php-ext-configure gd \
    --with-freetype \
    --with-jpeg

RUN docker-php-ext-install gd

# Fix PHP-FPM listen (IMPORTANT)
RUN sed -i 's|127.0.0.1:9000|0.0.0.0:9000|' /usr/local/etc/php-fpm.d/www.conf

# Install Redis
RUN apk add autoconf && pecl install -o -f redis \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable redis \
    && apk del autoconf

# Copy config
COPY ./config/php/local.ini /usr/local/etc/php/conf.d/local.ini

# Create user
RUN addgroup -g 1000 -S www && \
    adduser -u 1000 -S www -G www

# Set permissions
RUN chown -R www:www /var/www

USER www

# Copy project
COPY --chown=www:www . /var/www

EXPOSE 9000

CMD ["php-fpm"]