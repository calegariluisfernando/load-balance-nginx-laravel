FROM php:8.2-fpm

LABEL maintainer="calegariluisfernando@gmail.com"

# Arguments defined in docker-compose.yml
ARG user
ARG uid

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    nano \
    zip \
    unzip \
    bash \
    sudo \
    default-mysql-client

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-configure zip
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd zip opcache
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

RUN echo "memory_limit=8196M" >> /usr/local/etc/php/php.ini
RUN echo "max_execution_time = 300" >> /usr/local/etc/php/php.ini

RUN apt-get install ca-certificates
RUN update-ca-certificates

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Create system user to run Composer and Artisan Commands
RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer
RUN chown -R $user:$user /home/$user
RUN chown -R $user:$user /var/www

USER $user

# Set working directory
WORKDIR /var/www

CMD ["php-fpm"]