FROM php:8.2-fpm-alpine

# Build arguments
ARG PHP_REDIS_VERION="6.0.2"
ARG PHP_YAML_VERION="2.2.3"

ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so php

# Custom PHP requirements
RUN set -x \
    && mkdir -p /data \
    && apk --update add tar curl openssl sed libbz2 libxslt bzip2 libzip libuuid postgresql-dev icu-dev icu-data-full gettext-dev curl-dev libxml2-dev openldap-dev libpng libjpeg-turbo yaml libuuid oniguruma-dev gnu-libiconv libwebp-dev git \
    && apk add --virtual .phpize-deps $PHPIZE_DEPS linux-headers libxslt-dev libzip-dev bzip2-dev libmcrypt-dev imagemagick-dev libtool freetype-dev libpng-dev libjpeg-turbo-dev yaml-dev
RUN docker-php-ext-configure gd \
      --with-freetype \
      --with-jpeg \
      --with-webp
RUN docker-php-ext-install \
      gd \
      pdo_mysql \
      pdo_pgsql \
      opcache \
      intl \
      exif \
      gettext \
      bcmath \
      bz2 \
      pcntl \
      xsl \
      soap \
      sockets \
    && docker-php-ext-configure zip \
    && docker-php-ext-install zip \
    && docker-php-ext-configure ldap --with-libdir=lib/
RUN docker-php-ext-install ldap
RUN pecl install imagick \
      redis-${PHP_REDIS_VERION} \
      yaml-${PHP_YAML_VERION} \
      uuid
RUN docker-php-ext-enable imagick \
    && docker-php-ext-enable redis \
    && docker-php-ext-enable yaml \
    && docker-php-ext-enable uuid \
    && apk add --virtual .imagick-runtime-deps imagemagick \
    && apk del .phpize-deps

# install composer
ENV COMPOSER_ALLOW_SUPERUSER 1
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Configure PHP
RUN echo "date.timezone=${PHP_TIMEZONE:-UTC}" > $PHP_INI_DIR/conf.d/date_timezone.ini \
    && echo "memory_limit=${PHP_MEMORY_LIMIT:-512M}" > $PHP_INI_DIR/conf.d/memory_limit.ini \
    && echo "upload_max_filesize=${PHP_UPLOAD_MAX_FILESIZE:-256M}" > $PHP_INI_DIR/conf.d/upload_max_filesize.ini \
    && echo "post_max_size=${PHP_UPLOAD_MAX_FILESIZE:-256M}" > $PHP_INI_DIR/conf.d/post_max_size.ini \
    && echo "max_execution_time=${PHP_MAX_EXECUTION_TIME:-240}" > $PHP_INI_DIR/conf.d/max_execution_time.ini \
    && echo "max_input_vars=${PHP_MAX_INPUT_VARS:-1500}" > $PHP_INI_DIR/conf.d/max_input_vars.ini

# Define working directory
WORKDIR /data
