# FROM php:8.0-apache-buster
FROM mediawiki:latest

ENV COMPOSER_ALLOW_SUPERUSER=1

# ARG WWW_USER_ID=${WWW_USER_ID}
# ARG WWW_GROUP_ID=${WWW_GROUP_ID}
# 
# RUN usermod -u $WWW_USER_ID -o www-data
# RUN groupmod -g $WWW_GROUP_ID -o www-data

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev vim \
        libicu-dev \
        libzip-dev \
        libonig-dev \
        zip \
        unzip \
        libssl-dev  && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
        docker-php-ext-configure gd --with-freetype --with-jpeg \
        && docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-install intl mbstring opcache pdo_mysql zip

# COPY ./php.ini /usr/local/etc/php/php.ini
RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini 
COPY ./php.ini /usr/local/etc/php/conf.d/local-php.ini

# RUN pecl install apcu && docker-php-ext-enable apcu

## httpd
RUN mkdir /etc/apache2/include/
RUN echo 'Include include/*.conf ' >> /etc/apache2/apache2.conf

COPY ./*.conf /etc/apache2/include/

# sqlite data
# RUN mkdir /var/www/data
# RUN chgrp www-data /var/www/data
# RUN chmod g+w /var/www/data


WORKDIR /var/www/html

## network
# RUN docker network create sample-network
