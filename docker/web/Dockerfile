FROM php:8.2-apache

# backend
COPY ./php/php.ini /usr/local/etc/php/
COPY ./apache/*.conf /etc/apache2/sites-enabled/

ENV TZ Asia/Tokyo
RUN apt-get update \
  && apt-get install -y zlib1g-dev libpq-dev mariadb-client zip unzip \
  # BCMath
  && docker-php-ext-install bcmath \
  # GD
  && apt-get install -y zlib1g-dev libpng-dev libjpeg62-turbo-dev \
  && docker-php-ext-configure gd --with-jpeg \
  && docker-php-ext-install -j$(nproc) gd \
  # ZIP
  && apt-get install -y libzip-dev \
  && docker-php-ext-install zip \
  # 拡張モジュール
  && docker-php-ext-install pdo_mysql mysqli \
  && docker-php-ext-enable mysqli \
  # pgsql(supabase)
  && docker-php-ext-install pgsql pdo_pgsql \
  # mod_rewrite有効化
  && a2enmod rewrite

# npm
RUN apt-get update && apt-get install -y curl gnupg
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash -
RUN apt-get update && apt-get install -y nodejs
RUN npm install npm@latest

# composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

# laravel
WORKDIR /var/www/html
RUN composer global require "laravel/installer"

RUN mkdir -p /var/www/html/storage/framework/cache/data && \
  mkdir -p /var/www/html/storage/framework/sessions && \
  mkdir -p /var/www/html/storage/framework/views && \
  mkdir -p /var/www/html/storage/framework/testing && \
  chmod -R 777 /var/www/html/storage