ARG PHP_VERSION=8.1
ARG CONTAINER_REGISTRY=ghcr.io/syneido
ARG APP_ENV=prod


FROM ${CONTAINER_REGISTRY}/build-php${PHP_VERSION}:latest AS php_builder

WORKDIR /app

ENV APP_ENV=prod
ENV APP_DEBUG=0

COPY composer.json ./
RUN set -eux; \
	composer install --prefer-dist --no-dev --no-autoloader --no-scripts --no-progress --no-suggest; \
	composer clear-cache

# copy only what we need
COPY .env ./
COPY public public/

RUN set -eux; sync

# PHP-FPM Stage
FROM ${CONTAINER_REGISTRY}/${APP_ENV}-php${PHP_VERSION}-fpm:latest AS php_fpm
WORKDIR /var/www
COPY --from=php_builder --chown=www-data:www-data /app  .
COPY docker/php/conf.d/zz_01-additional.prod.ini /usr/local/etc/php/conf.d/zz_01-additional.ini
#REMOVE COMMENT IF YOU NEED ENTRYPOINT
#COPY docker/docker-entrypoint /usr/local/bin/docker-php-entrypoint
#RUN chmod +x /usr/local/bin/docker-php-entrypoint;

# Nginx Stage
FROM ${CONTAINER_REGISTRY}/dev-nginx:latest AS nginx
WORKDIR /var/www
COPY --from=php_builder /app/public  ./public
COPY docker/nginx/conf/nginx.conf.template /etc/nginx/templates/nginx.conf
COPY docker/nginx/conf/project.conf.template /etc/nginx/templates/project.conf.template