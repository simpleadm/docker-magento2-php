FROM php:7.1-cli-alpine

# Install PHP extensions
RUN set -eux; \
    \
    apk add --no-cache --virtual .build-deps \
        coreutils \
        freetype-dev \
        icu-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        libxslt-dev \
        libzip-dev \
    ; \
    \
    docker-php-ext-configure gd \
        --with-freetype-dir=/usr/include \
        --with-jpeg-dir=/usr/include \
        --with-png-dir=/usr/include \
    ; \
    \
    docker-php-ext-install -j "$(nproc)" \
        bcmath \
        gd \
        intl \
        opcache \
        pdo_mysql \
        soap \
        xsl \
        zip \
    ; \
    \
    runDeps="$( \
        scanelf --needed --nobanner --format '%n#p' --recursive /usr/local \
            | tr ',' '\n' \
            | sort -u \
            | awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
    )"; \
    apk add --virtual .magento-phpexts-rundeps $runDeps; \
    apk del .build-deps


# Install composer
RUN curl -sS https://getcomposer.org/installer | \
  php -- --install-dir=/usr/local/bin --filename=composer

CMD ["php", "-a"]
