FROM php:7.2-cli

RUN rm -rf /var/lib/apt/lists/* && \
    apt-get update && \
    apt-get install --yes \
    ssmtp \
	apt-transport-https \
	lsb-release \
	ca-certificates \
	wget && \
    wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg && \
    echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list && \
    apt-get update && \
    apt-get dist-upgrade --yes

RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libicu-dev \
    libicu57 \
    libsodium-dev \
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
    pdo_mysql \
    soap \
    sodium \
    xsl \
    zip

RUN pecl install xdebug-2.7.0 && docker-php-ext-enable xdebug

RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

RUN mv "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini" "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini.disabled"

COPY files/custom-php.ini $PHP_INI_DIR/conf.d/