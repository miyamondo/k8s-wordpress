FROM php:8.3-fpm-alpine
ENV APP_ROOT /var/www/dev-k8s.menta.me
WORKDIR $APP_ROOT

# Setup UTC+9
RUN apk --update add tzdata && \
    cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
    apk del tzdata && \
    rm -rf /var/cache/apk/*

# install packages
RUN apk update && \
    apk upgrade && \
    apk add --update --no-cache \
    autoconf \
    bash \
    build-base \
    curl-dev \
    freetype-dev \
    g++ \
    gcc \
    git \
    libjpeg-turbo-dev \
    libpng-dev \
    libxml2-dev \
    libxslt-dev \
    make \
    mysql-dev \
    mysql-client \
    nginx \
    openssl \
    py3-pip \
    shadow \
    sudo \
    supervisor \
    tzdata \
    vim \
    yaml-dev \
    zlib-dev \
    linux-headers \
    wget \
    tar \
    curl

# PHP 拡張のインストール
RUN docker-php-ext-install pdo_mysql mysqli soap

RUN docker-php-ext-configure gd \
    --with-freetype=/usr/include/ \
    --with-jpeg=/usr/include/ && \
    NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
    docker-php-ext-install -j${NPROC} gd

# PHP ユーザーとグループの設定
RUN addgroup -S nginx || true
RUN adduser -S nginx -G nginx || true

# ディレクトリの作成と所有者の変更
RUN mkdir -p $APP_ROOT
RUN mkdir -p $APP_ROOT/wp-content/cache
RUN mkdir -p $APP_ROOT/wp-content/upgrade
RUN mkdir -p $APP_ROOT/wp-content/uploads
RUN chown -R nginx:nginx $APP_ROOT
RUN mkdir -p /var/run/php-fpm
RUN rm -f /usr/local/etc/php-fpm.conf.default
RUN rm -f /usr/local/etc/php-fpm.d/zz-docker.conf
COPY ./php-fpm.conf /usr/local/etc/php-fpm.conf
COPY ./php.ini /usr/local/etc/php/php.ini
COPY ./www.conf /usr/local/etc/php-fpm.d/www.conf
COPY ./wordpress/. $APP_ROOT

# Install xdebug
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

# composer install
RUN curl -sS https://getcomposer.org/installer | php -- --version=2.2.6 --install-dir=/usr/bin
RUN mv /usr/bin/composer.phar /usr/bin/composer

# Install PHP dotenv
RUN composer require vlucas/phpdotenv

CMD ["/usr/local/sbin/php-fpm", "-F"]
