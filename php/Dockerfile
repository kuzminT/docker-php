# ./Dockerfile
FROM php:5.4-fpm

#LABEL maintainer="pierstoval@gmail.com"

## Not mandatory, but I use it as a convention, it's easier to set it up for any other project
WORKDIR /var/www/html/

## Having it named as "99-..." makes sure your file is the last one to be loaded,
## therefore helping you override any part of PHP's native config.
COPY ./etc/php.ini /usr/local/etc/php/conf.d/99-custom.ini
#COPY ../srv/app /var/www/html/

RUN set -xe \
    && apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        make \
        curl \
        vim \
        less \
	wget \
	git \
libfreetype6-dev \
libjpeg62-turbo-dev \
libxslt-dev \
libicu-dev \
libmcrypt-dev \
libpng12-dev \
libxml2-dev \
        unzip \
&& docker-php-ext-install iconv mcrypt mbstring mysqli pdo_mysql zip \
&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
&& docker-php-ext-install gd \
&& `# Clean apt cache and remove unused libs/packages to make image smaller` \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false -o APT::AutoRemove::SuggestsImportant=false $BUILD_LIBS \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/www/* /var/cache/*
    

RUN chown -R www-data:www-data /var/www/html/
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
