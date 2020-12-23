FROM php:7.4-fpm

RUN docker-php-ext-install pdo_mysql

ENV DOCKERIZE_VERSION 0.6.1

# Install dockerize so we can wait for containers to be ready
RUN curl -s -f -L -o /tmp/dockerize.tar.gz https://github.com/jwilder/dockerize/releases/download/v$DOCKERIZE_VERSION/dockerize-linux-amd64-v$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf /tmp/dockerize.tar.gz \
    && rm /tmp/dockerize.tar.gz

# Install nodejs
RUN curl -sL https://deb.nodesource.com/setup_12.x | bash

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        git \
        vim \
        libmemcached-dev \
        libz-dev \
        libpq-dev \
        libjpeg-dev \
        libpng-dev \
        libfreetype6-dev \
        libssl-dev \
        libmcrypt-dev \
        libzip-dev \
        unzip \
        zip \
        nodejs \
    && docker-php-ext-configure gd \
    && docker-php-ext-configure zip \
    && docker-php-ext-install \
        gd \
        exif \
        opcache \
        pdo_mysql \
        pdo_pgsql \
        pcntl \
        zip \
    && rm -rf /var/lib/apt/lists/*;

# Clear cache
#RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql exif pcntl bcmath gd

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Create system user to run Composer and Artisan Commands
#RUN useradd -G www-data,root -u $uid -d /home/$user $user
#RUN mkdir -p /home/$user/.composer && \
#    chown -R $user:$user /home/$user

# Set working directory
WORKDIR /var/www

USER $user

#COPY ./laravel.ini /usr/local/etc/php/conf.d/laravel.ini
