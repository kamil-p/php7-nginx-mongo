FROM php:7.0.8-fpm

ARG USERID=1000

RUN usermod -u $USERID www-data && \
    mkdir -p /var/www && \
    chown -R www-data:www-data /var/www && \
    chmod 777 -R /var/www

RUN apt-get -qq update --fix-missing && \
    apt-get -q install -y \
        git \
        zlib1g-dev libicu-dev g++ \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libxml2-dev libxslt1-dev \
        libc-client-dev libkrb5-dev \
        cloc \
        default-jdk ant

RUN docker-php-ext-configure imap --with-imap-ssl --with-kerberos \
    && docker-php-ext-install iconv intl mysqli gd mbstring pdo_mysql xsl opcache imap bcmath zip

RUN curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer

# Install mongo
RUN  pecl install mongodb \
	&& pecl install xdebug-beta \
	&& docker-php-ext-enable xdebug

RUN echo "extension=mongodb.so" > $PHP_INI_DIR/conf.d/mongodb.ini

COPY ./config/php.ini /usr/local/etc/php
ADD ./config/entrypoint.sh /entrypoint.sh

VOLUME /var/www
WORKDIR /var/www
USER www-data

#ENTRYPOINT ["/bin/bash", "/entrypoint.sh"]