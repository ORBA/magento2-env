FROM php:7.1-cli

RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libicu-dev \
    libicu57 \
    libmcrypt-dev \
    libmcrypt4 \
    libxml2-dev \
    libxml2 \
    libxslt1-dev \
    libxslt1.1 \
    zlib1g-dev \
    zlib1g

RUN docker-php-ext-install -j$(nproc) \
    bcmath \
    gd \
    intl \
    mcrypt \
    pdo_mysql \
    soap \
    xsl \
    zip

RUN pecl install xdebug-2.7.0 && docker-php-ext-enable xdebug

RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

RUN mv "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini" "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini.disabled"

COPY config/custom-php.ini $PHP_INI_DIR/conf.d/