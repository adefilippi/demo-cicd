ARG PHP_VERSION=8.1
ARG CONTAINER_REGISTRY=ghcr.io/syneido
ARG APP_ENV=prod
ARG NODE_VERSION=14

FROM ${CONTAINER_REGISTRY}/build-node:${NODE_VERSION} AS node_builder
WORKDIR /app

COPY package.json yarn.lock webpack.config.js ./
COPY public public/
COPY src src/
COPY assets assets/

RUN set -eux; \
    yarn install; \
    yarn run build

FROM ${CONTAINER_REGISTRY}/build-php${PHP_VERSION}:latest AS php_builder

WORKDIR /app

ENV APP_ENV=prod
ENV APP_DEBUG=0

COPY composer.json composer.lock symfony.lock ./
RUN set -eux; \
	composer install --prefer-dist --no-dev --no-autoloader --no-scripts --no-progress --no-suggest; \
	composer clear-cache

# copy only what we need
COPY .env ./
COPY bin bin/
COPY config config/
COPY migrations migrations/
COPY translations translations/
COPY public public/
COPY templates templates/
COPY src src/
COPY --from=node_builder /app/public/ ./public

RUN set -eux; \
	mkdir -p var/cache var/log; \
	composer dump-autoload --classmap-authoritative --no-dev; \
	composer dump-env prod; \
	composer run-script --no-dev post-install-cmd; \
	chmod +x bin/console; sync

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