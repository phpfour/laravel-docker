FROM php:7.4.4-fpm

# @see https://floor12.net/en/blog%2Fperfect-php-fpm-image.html

# Arguments defined in docker-compose.yml
ARG USER_ID
ARG GROUP_ID

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libcurl4-openssl-dev \
    libzip-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip

# Install PHP extensions (keep in sync them with composer.json required expentions [expect curl dom hash filter json libxml pdo])
RUN docker-php-ext-install pdo_mysql pcntl bcmath

# Install ext-gd
RUN apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libwebp-dev \
    libfreetype6-dev
RUN docker-php-ext-configure gd \
    --with-webp=/usr/include/ \
    --with-freetype=/usr/include/ \
    --with-jpeg=/usr/include/
RUN docker-php-ext-install gd

# Install ext-zip
RUN docker-php-ext-configure zip
RUN docker-php-ext-install zip

# Install ext-intl
RUN docker-php-ext-configure intl && docker-php-ext-install -j$(nproc) intl

# Install ext-redis
RUN pecl install redis && docker-php-ext-enable redis

# https://blog.denisbondar.com/post/phpstorm_docker_xdebug (RU)
RUN pecl install xdebug && docker-php-ext-enable xdebug

# https://www.hmazter.com/2019/04/speeding-up-php-docker-with-opcache/
RUN docker-php-ext-install opcache

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Create system user to run Composer and Artisan Commands
RUN useradd -G www-data,root -u $GROUP_ID -d /home/$USER_ID $USER_ID
RUN mkdir -p /home/$USER_ID/.composer && \
    chown -R $USER_ID:$USER_ID /home/$USER_ID

# Set working directory
WORKDIR /var/www

USER $USER_ID
