# Stage 1: Install dependencies with Composer
FROM composer:latest AS builder

WORKDIR /app

COPY . .

RUN composer install --no-dev --optimize-autoloader

# Stage 2: PHP-Apache image
FROM php:8.2-apache

# Install system dependencies
RUN apt-get update && apt-get install -y \
    libzip-dev zip unzip && \
    docker-php-ext-install pdo pdo_mysql zip

# Copy Laravel project from builder
COPY --from=builder /app /var/www/html

# Set proper document root
ENV APACHE_DOCUMENT_ROOT=/var/www/html/public

# Adjust Apache config
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf \
 && sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

# Enable mod_rewrite
RUN a2enmod rewrite

# Set permissions (storage + cache folder)
RUN chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache
# Tambahkan file SQLite (jika belum ada) dan beri permission
RUN touch /var/www/html/database/database.sqlite && \
    chmod -R 775 /var/www/html/database /var/www/html/storage /var/www/html/bootstrap/cache && \
    chown -R www-data:www-data /var/www/html


EXPOSE 80

CMD ["apache2-foreground"]
