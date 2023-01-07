FROM php:8.2-fpm

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get -y update --fix-missing && \
    apt-get upgrade -y && \
    apt-get --no-install-recommends install -y apt-utils && \
    rm -rf /var/lib/apt/lists/*

RUN apt-get -y update && \
    apt-get -y --no-install-recommends install \
    nano \
    wget \
    dialog \
    git \
    curl \
    libsqlite3-dev \
    zip \
    unzip \
    libssl-dev \
    libsqlite3-0 && \
    apt-get -y --no-install-recommends install \
    default-mysql-client \
    zlib1g-dev \
    libzip-dev \
    libicu-dev && \
    apt-get -y --no-install-recommends install --fix-missing \
    build-essential \
    libonig-dev && \
    apt-get -y --no-install-recommends install --fix-missing \
    libcurl4 \
    libcurl4-openssl-dev \
    libmcrypt-dev \
    openssl && \
    apt-get -y --no-install-recommends install --fix-missing \
    imagemagick \
    libfreetype6-dev \
    libmagickwand-dev \
    libjpeg-dev \
    libjpeg62-turbo-dev \
    libpng-dev && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get autoremove --purge -y && \
    apt-get autoclean -y && \
    apt-get clean -y

RUN pecl install xdebug && \
    docker-php-ext-enable xdebug

RUN pecl install redis && \
    docker-php-ext-enable redis

RUN docker-php-ext-install pdo_mysql && \
    docker-php-ext-install zip && \
    docker-php-ext-install iconv && \
    docker-php-ext-install intl && \
    docker-php-ext-install mbstring && \
    docker-php-ext-install gettext && \
    docker-php-ext-install gd && \
    docker-php-ext-configure intl && \
    pecl install imagick && \
    docker-php-ext-enable imagick opcache


RUN mkdir -p /var/log/php-fpm
RUN ln -sf /dev/stdout /var/log/php-fpm/access.log
RUN ln -sf /dev/stderr /var/log/php-fpm/error.log

ARG uid
ARG gid
ARG uname

RUN groupadd --gid $gid $uname
RUN useradd -G www-data,root -s /bin/bash --uid $uid --gid $gid $uname --home-dir /var/www

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && chown $uid:$gid /usr/local/bin/composer

RUN mkdir -p /var/www/.composer
RUN mkdir -p /var/www/.composer/cache
RUN chown -R $uid:$gid /var/www
RUN chown -R $uid:$gid /var/www/.composer

RUN mkdir /tmp/xhprof \
    && chown $uid:$gid /tmp/xhprof

ENV PHP_EXTRA_CONFIGURE_ARGS --enable-fpm --with-fpm-user=$uid --with-fpm-group=$gid

WORKDIR /var/www/html

CMD ["php-fpm"]

RUN rm -rf /usr/src/*

USER $uname
