FROM spiralscout/roadrunner:2023.3.1 AS roadrunner
FROM php:8.2.11-cli-bookworm

COPY --from=roadrunner /usr/bin/rr /usr/bin/rr
COPY --from=composer /usr/bin/composer /usr/bin/composer

RUN apt-get update -y && \
    apt-get install -y --no-install-recommends \
        git \
        libmariadb-dev \
        libxml2-dev \
        libzip-dev
RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && \
    apt-get install -y nodejs
RUN apt-get autoremove -y && \
    apt-get clean && \
	rm -rf /var/lib/apt/lists/*
RUN pecl install \
    xdebug-3.2.2
RUN docker-php-ext-install \
    pcntl \
    pdo_mysql \
    sockets \
    xml \
    zip
RUN docker-php-ext-enable \
    xdebug
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"
ARG PHP_UID=1000
ARG PHP_USER=vscode
ARG PHP_HOME=/home/vscode
ARG WORKSPACE_DIR=/workspace
RUN useradd -g www-data -u ${PHP_UID} -d ${PHP_HOME} -m ${PHP_USER}
