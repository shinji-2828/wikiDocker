FROM mediawiki:latest

## PHP
ENV COMPOSER_ALLOW_SUPERUSER=1
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

RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini 
COPY ./php.ini /usr/local/etc/php/conf.d/local-php.ini

## httpd
RUN mkdir /etc/apache2/include/
RUN echo 'Include include/*.conf ' >> /etc/apache2/apache2.conf
COPY ./*.conf /etc/apache2/include/

WORKDIR /var/www/html
