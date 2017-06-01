FROM php:7-fpm

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng12-dev \
        libpq-dev \
        g++ \
        libicu-dev \
        libxml2-dev \
    && docker-php-ext-configure intl \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install intl \
    && docker-php-ext-install zip \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install pdo_pgsql \
    && docker-php-ext-install soap \
    && apt-get purge --auto-remove -y g++ \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*


# install php-redis
ENV PHPREDIS_VERSION php7

RUN curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz  \
    && mkdir /tmp/redis \
    && tar -xf /tmp/redis.tar.gz -C /tmp/redis \
    && rm /tmp/redis.tar.gz \
    && ( \
    cd /tmp/redis/phpredis-$PHPREDIS_VERSION \
    && phpize \
        && ./configure \
    && make -j$(nproc) \
        && make install \
    ) \
    && rm -r /tmp/redis \
    && docker-php-ext-enable redis



# install GD and mcrypt
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

# install apcu
RUN pecl install apcu \
    && docker-php-ext-enable apcu

#install Imagemagick & PHP Imagick ext
RUN apt-get update && apt-get install -y \
        libmagickwand-dev --no-install-recommends

RUN pecl install imagick && docker-php-ext-enable imagick

# install mongodb ext
RUN pecl install mongodb \
    && docker-php-ext-enable mongodb

# install git
RUN apt-get update && apt-get install git git-core -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

#install xdebug
RUN pecl install xdebug && docker-php-ext-enable xdebug
RUN echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

#Other PHP Settings
RUN echo "upload_max_filesize = 100M" >> /usr/local/etc/php/conf.d/docker-php-general.ini
RUN echo "post_max_size = 100M" >> /usr/local/etc/php/conf.d/docker-php-general.ini
RUN echo "date.timezone = Europe/Rome" >> /usr/local/etc/php/conf.d/docker-php-general.ini

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Permissions
RUN usermod -u 1000 www-data

# Set timezone
RUN echo "Europe/Rome" > /etc/timezone
RUN dpkg-reconfigure -f noninteractive tzdata

# Cleanup
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Add Symfony alias
RUN echo 'alias sf="php bin/console"' >> ~/.bashrc

# Add Symfony extra commands
#RUN chown -R www-data:www-data /app/var/cache
#RUN chown -R www-data:www-data /app/var/logs

WORKDIR /app