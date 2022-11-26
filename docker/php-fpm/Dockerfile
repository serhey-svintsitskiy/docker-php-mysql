FROM php:8.1-fpm


RUN apt-get update \
    && apt-get install -y --no-install-recommends -f iproute2 imagemagick libmagickwand-dev libicu-dev libzip-dev zip libpng-dev libmemcached-dev \
    && rm -rf /var/lib/apt/lists/* && apt-get clean

RUN curl -s https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && curl -sL https://phar.phpunit.de/phpunit-9.phar --output phpunit \
    && chmod +x phpunit && mv phpunit /usr/local/bin/phpunit

RUN pecl install memcached xdebug imagick
RUN docker-php-ext-configure intl
RUN docker-php-ext-install intl zip pdo pdo_mysql gd zip bcmath sockets exif
RUN docker-php-ext-enable xdebug pdo_mysql imagick

RUN echo "\nxdebug.idekey=PHPSTORM\nxdebug.mode=debug" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "\nxdebug.client_host="`/sbin/ip route|awk '/default/ { print $3 }'` >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "\nxdebug.discover_client_host=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "\nxdebug.start_with_request=yes" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN usermod -u 1000 www-data;

WORKDIR /opt/project
