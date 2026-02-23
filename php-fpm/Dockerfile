FROM php:8.4.14-fpm-bookworm

### Installing required packages and removing useless ones (to fix CVEs)
RUN apt-get update -y &&\
    apt-get upgrade -y &&\
    apt-get purge -y \
        wget &&\
    apt-get install -y --no-install-recommends \
        default-mysql-client \
        libfcgi-bin \
        msmtp \
        libcurl4-openssl-dev \
        ghostscript \
        libzip-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        imagemagick \
        libmagickwand-dev \
        libpng-dev &&\
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Installing PHP extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-install mysqli && \
    docker-php-ext-install -j$(nproc) gd && \
    docker-php-ext-install pdo_mysql && \
    docker-php-ext-install zip && \
    docker-php-ext-install curl && \
    pecl install imagick && \
    docker-php-ext-enable imagick

# Installing REDCap webapp
RUN rm -rf /var/www/html

RUN mkdir -p /app/redcap && \
    ln -s /app/redcap /var/www/redcap

RUN chmod 775 -R /var/www/redcap && \
    chown -R www-data:www-data /app/redcap && \
    chown -R www-data:www-data /var/www/redcap

RUN mkdir /edocs \
    && chmod 775 -R /edocs \
    && chown -R www-data:www-data /edocs

WORKDIR /var/www/redcap

# Switching to app user
USER www-data

### Starting PHP-FPM service ###
CMD ["php-fpm"]

