FROM php:8.1-fpm-alpine

# Cài đặt các gói và PHP extensions cần thiết
RUN apk add --no-cache \
    git \
    curl \
    libpng-dev \
    libjpeg-turbo-dev \
    freetype-dev \
    libzip-dev \
    zip \
    unzip \
    icu-dev \
    bash \
    postgresql-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) pdo pdo_mysql pdo_pgsql gd zip intl bcmath

# Cài đặt Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Thiết lập thư mục làm việc
WORKDIR /var/www/html

# Copy file composer.lock và composer.json
COPY ./laravel_src/composer.lock ./laravel_src/composer.json /var/www/html/

# Cài đặt dependencies từ composer
RUN composer install --prefer-dist --no-scripts --no-dev --optimize-autoloader

# Copy mã nguồn
# COPY ./laravel_src /var/www/html

# Phân quyền
RUN chown -R www-data:www-data /var/www/html
RUN chmod -R 777 /var/www/html

# Chạy các lệnh artisan cần thiết
# RUN php artisan config:cache
# RUN php artisan route:cache
# RUN php artisan view:cache