FROM php:${PHP_VERSION:-8.1}-fpm

# Set working directory
WORKDIR /var/www/main

# Install dependencies
RUN apt-get update && apt-get install -y \
apt-transport-https \
build-essential \
libpng-dev \
libjpeg62-turbo-dev \
libfreetype6-dev \
locales \
libzip-dev \
zip \
jpegoptim optipng pngquant gifsicle \
vim \
unzip \
git \
nano \
libssl-dev \
libcurl4-openssl-dev \
libicu-dev \
libmcrypt-dev \
libxslt-dev \
libc-client-dev \
libkrb5-dev \
&& apt-get clean && rm -rf /var/lib/apt/lists/*c \
&& docker-php-ext-install pdo_mysql zip exif pcntl \
&& docker-php-ext-configure gd \
&& docker-php-ext-install gd \
&& pecl channel-update pecl.php.net \
&& docker-php-ext-install bcmath \
&& docker-php-ext-install calendar \
&& docker-php-ext-install curl \
&& docker-php-ext-install gettext \
&& docker-php-ext-install intl \
&& docker-php-ext-install -j$(nproc) intl \
&& docker-php-ext-install mysqli \
&& docker-php-ext-install shmop \
&& docker-php-ext-install sockets \
&& docker-php-ext-install sysvmsg \
&& docker-php-ext-install sysvsem \
&& docker-php-ext-install sysvshm \
&& docker-php-ext-install xsl \
&& docker-php-ext-install xml \
&& docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
&& docker-php-ext-install imap

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Add user for laravel application
RUN groupadd -g 1000 www && useradd -u 1000 -ms /bin/bash -g www www

# Add command aliases
RUN echo 'alias artisan="php artisan"' >> /home/www/.bash_aliases

# Copy existing application directory contents
COPY ./backend /var/www/main

# Copy existing application directory permissions
COPY --chown=www:www . /var/www/main

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
