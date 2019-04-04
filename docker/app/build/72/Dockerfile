ARG PHP_EXTENSIONS="bcmath gd intl mcrypt opcache pdo_mysql soap xdebug xsl zip"
ARG INSTALL_CRON=1

FROM thecodingmachine/php:7.2-v2-slim-apache

USER root

RUN apt-get update && apt-get install ssmtp -y

COPY files/auth.json /var/www/auth.json
COPY files/magento2local.crt /etc/apache2/ssl/magento2local.crt
COPY files/magento2local.key /etc/apache2/ssl/magento2local.key
COPY files/magento2root.pem /etc/apache2/ssl/magento2root.pem
COPY files/000-default.conf /etc/apache2/sites-available/000-default.conf
COPY files/ssmtp.conf /etc/ssmtp/ssmtp.conf

USER docker

ENV APACHE_EXTENSION_SOCACHE_SHMCB=1 \
    APACHE_EXTENSION_SSL=1 \
    PHP_EXTENSION_BCMATH=1 \
    PHP_EXTENSION_GD=1 \
    PHP_EXTENSION_INTL=1 \
    PHP_EXTENSION_OPCACHE=1 \
    PHP_EXTENSION_PDO_MYSQL=1 \
    PHP_EXTENSION_SOAP=1 \
    PHP_EXTENSION_XSL=1 \
    PHP_EXTENSION_ZIP=1 \
    PHP_INI_MEMORY_LIMIT=2048M \
    PHP_INI_ERROR_REPORTING=E_ALL \
    PHP_INI_OPCACHE__MEMORY_CONSUMPTION=256 \
    PHP_INI_OPCACHE__INTERNET_STRINGS_BUFFER=12 \
    PHP_INI_OPCACHE__MAX_ACCELERATED_FILES=65406 \
    PHP_INI_OPCACHE__ENABLE_FILE_OVERRIDE=1 \
    PHP_INI_OPCACHE__REVALIDATE_FREQ=0 \
    PHP_INI_SENDMAIL_PATH="/usr/sbin/ssmtp -t"