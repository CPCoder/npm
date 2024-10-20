FROM alpine:3.20
# Setup document root
WORKDIR /var/www/main

# Install packages and remove default server definition
RUN apk update && apk upgrade && apk add --no-cache \
  curl \
  nginx \
  php83 \
  php83-ctype \
  php83-curl \
  php83-dom \
  php83-fileinfo \
  php83-fpm \
  php83-gd \
  php83-intl \
  php83-mbstring \
  php83-mysqli \
  php83-opcache \
  php83-openssl \
  php83-phar \
  php83-session \
  php83-tokenizer \
  php83-xml \
  php83-xmlreader \
  php83-xmlwriter \
  supervisor

# Config PHP
RUN sed -i "s/user = nobody/user = root/g" /etc/php83/php-fpm.d/www.conf \
    && sed -i "s/group = nobody/group = root/g" /etc/php83/php-fpm.d/www.conf \
    && sed -i "s/listen.owner = nobody/listen.owner = root/g" /etc/php83/php-fpm.d/www.conf \
    && sed -i "s/listen.group = nobody/listen.group = root/g" /etc/php83/php-fpm.d/www.conf \
    && sed -i "s/listen.group = nobody/listen.group = root/g" /etc/php83/php-fpm.d/www.conf

# Install Composer
RUN curl -sS https://getcomposer.org/installer | /usr/bin/php -- --install-dir=/usr/bin --filename=composer

# Configure nginx
COPY config/nginx.conf /etc/nginx/nginx.conf
COPY config/conf.d /etc/nginx/conf.d/
COPY config/ssl /etc/nginx/ssl

# Configure PHP-FPM
COPY config/fpm-pool.conf /etc/php83/php-fpm.d/www.conf
COPY config/php.ini /etc/php8/conf.d/custom.ini

# Configure supervisord
COPY config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Create www-data user -- for some reason, the group already existed
# We use UID 1000 to match the host user (so volume permissions automatically line up).
#   I don't like this, it feels like it shouldn't be something hardcoded, but I haven't found a better option
RUN adduser -D -H -u 1000 -s /bin/bash www-data -G www-data

# Make sure files/folders needed by the processes are accessable when they run under the nobody user
# Same as original Dockerfile
RUN chown -R www-data.www-data /var/www/main /run /var/lib/nginx /var/log/nginx /etc/nginx/ssl

# Add application
COPY --chown=www-data:www-data . /var/www/main

# Expose the port nginx is reachable on
EXPOSE 80
EXPOSE 443
#EXPOSE 9000

# Let supervisord start nginx & php-fpm
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]

# Run supervisor as www-data so nginx and such are spawned by www-data, too.
USER www-data

# Configure a healthcheck to validate that everything is up&running
HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1/fpm-ping
