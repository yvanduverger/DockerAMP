FROM php:8.4-fpm-bookworm

ARG DEBIAN_FRONTEND=noninteractive

ENV COMPOSER_ALLOW_SUPERUSER=1 \
    COMPOSER_HOME=/tmp/composer

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        git \
        unzip \
        zip \
        libicu-dev \
        libzip-dev \
        libonig-dev \
        libpq-dev \
    && docker-php-ext-configure intl \
    && docker-php-ext-install -j"$(nproc)" \
        intl \
        mbstring \
        opcache \
        pdo_mysql \
        zip \
    && rm -rf /var/lib/apt/lists/*

# Installation de Composer depuis l'image officielle Composer
COPY --from=composer/composer:2-bin /composer /usr/local/bin/composer

# Installation de Symfony CLI
RUN curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' \
        | bash \
    && apt-get update \
    && apt-get install -y --no-install-recommends symfony-cli \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/

# Vérifications lors de la construction de l'image
RUN php -v \
    && composer --version \
    && symfony version \
    && git --version

COPY conf.d/timezone.ini /usr/local/etc/php/conf.d/99-timezone.ini
CMD ["php-fpm"]
