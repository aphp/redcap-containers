FROM php:8.2-fpm-bookworm

### Installing required packages
RUN apt-get update -y &&\
    apt-get -y --no-install-recommends install \
        net-tools  \
        vim \
        wget \
        curl \
        ghostscript \
        libcurl4-openssl-dev \
        zip \
        msmtp \
        libzip-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        imagemagick \
        libmagickwand-dev \
        libpng-dev \
        dos2unix && \
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

