FROM php:8.4-cli

RUN apt-get update \
    && apt-get install -y --no-install-recommends git unzip libzip-dev libicu-dev libpq-dev \
    && docker-php-ext-install pdo_pgsql zip intl \
    && rm -rf /var/lib/apt/lists/*

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# O container roda com o UID de quem desenvolve, que não tem home na imagem
ENV COMPOSER_HOME=/tmp/composer

WORKDIR /app
