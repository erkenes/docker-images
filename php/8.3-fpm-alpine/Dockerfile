FROM php:8.3-fpm-alpine

ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so php

# Custom PHP requirements
RUN set -x \
    && mkdir -p /data \
    && apk --update add tar curl openssl sed libbz2 libxslt bzip2 libzip libuuid postgresql-dev icu-dev icu-data-full gettext-dev curl-dev libxml2-dev openldap-dev libpng libwebp libjpeg-turbo yaml libuuid oniguruma-dev gnu-libiconv git \
    && apk add --virtual .phpize-deps $PHPIZE_DEPS linux-headers libxslt-dev libzip-dev bzip2-dev libmcrypt-dev imagemagick-dev libtool freetype-dev libpng-dev libjpeg-turbo-dev libwebp-dev yaml-dev
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

# Imagick is installed from the archive
ADD --chmod=0755 \
  https://github.com/mlocati/docker-php-extension-installer/releases/download/2.6.3/install-php-extensions \
  /usr/local/bin/
# TODO: Use latest released version, after https://github.com/Imagick/imagick/issues/640 is fixed
RUN install-php-extensions imagick/imagick@28f27044e435a2b203e32675e942eb8de620ee58
# <<< End of Imagick installation

RUN pecl install redis yaml uuid \
    && docker-php-ext-enable imagick \
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
