FROM --platform=linux/amd64 php:7.4-fpm
# Arguments defined in docker-compose.yml
# ARG user
# ARG uid
# Set working directory
# WORKDIR /var/www/html
# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip

RUN pwd
# RUN apt-get update && apt-get install -y \
#     software-properties-common \
#     npm
# RUN npm install npm@latest -g && \
#     npm install n -g && \
#     n latest
# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# Get latest Composer
COPY docker/ /var/www/html
COPY docker/entrypoint.sh /entrypoint.sh
COPY src/ /var/www/html
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
COPY /src /var/www/html

RUN chmod -R 765 /var/www/html/
RUN chmod +x /entrypoint.sh

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer install --no-interaction --optimize-autoloader

# Set the permissions for storage and bootstrap/cache directories
RUN chown -R www-data:www-data storage bootstrap/cache

# USER $user
EXPOSE 8000
ENTRYPOINT [ "/entrypoint.sh" ]

