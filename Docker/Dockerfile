#
# Dockerfile for opencart
#

FROM php:7.3.1-apache
MAINTAINER aaron <info@aamservices.uk>

RUN apt-get update && \
    apt-get install -y --no-install-recommends git zip libzip-dev

RUN curl --silent --show-error https://getcomposer.org/installer | php

RUN a2enmod rewrite

RUN set -xe \
    && apt-get update \
    && apt-get install -y libpng-dev libjpeg-dev libmcrypt-dev \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr \
    && docker-php-ext-install gd mbstring mysqli pdo_mysql zip \
    && pecl install mcrypt-1.0.2 \
    && docker-php-ext-enable mcrypt

WORKDIR /var/www/html

ENV OPENCART_VER 3.0.2.0
ENV OPENCART_URL https://github.com/opencart/opencart/archive/${OPENCART_VER}.tar.gz
ENV OPENCART_FILE opencart.tar.gz

RUN set -xe \
    && curl -sSL ${OPENCART_URL} -o ${OPENCART_FILE} \
    && tar xzf ${OPENCART_FILE} --strip 2 --wildcards '*/upload/' \
    && mv config-dist.php config.php \
    && mv admin/config-dist.php admin/config.php \
    && rm ${OPENCART_FILE} \
    && rm -rf system/storage/vendor \
	&& sed -i 's/MYSQL40//g' install/model/install/install.php \
    && chown -R www-data:www-data /var/www
