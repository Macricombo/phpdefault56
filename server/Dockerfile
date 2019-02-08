FROM php:5.6-apache

RUN apt-get update && apt-get install -y --no-install-recommends apt-utils

RUN apt-get install -y zlib1g-dev libmcrypt-dev libicu-dev libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
curl && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install pdo pdo_mysql mysql \
mcrypt intl mbstring gd zip

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

# Install Xdebug
RUN curl -fsSL 'https://xdebug.org/files/xdebug-2.4.0.tgz' -o xdebug.tar.gz \
&& mkdir -p xdebug \
&& tar -xf xdebug.tar.gz -C xdebug --strip-components=1 \
&& rm xdebug.tar.gz \
&& ( \
cd xdebug \
&& phpize \
&& ./configure --enable-xdebug \
&& make -j$(nproc) \
&& make install \
) \
&& rm -r xdebug \
&& docker-php-ext-enable xdebug

RUN echo "date.timezone=Europe/Rome" >> /usr/local/etc/php/php.ini \
&& echo "xdebug.remote_enable=1" >> /usr/local/etc/php/php.ini

RUN a2enmod rewrite headers

COPY 000-default.conf /etc/apache2/sites-available/000-default.conf

RUN usermod -u 1000 www-data && groupmod -g 1000 www-data

