FROM richarvey/nginx-php-fpm:1.5.7

ENV DRUSH_VERSION 0.6.0

##################################From Drupal#################################### 
# install the PHP extensions we need
RUN set -eux; \
    apk update && apk upgrade &&\
    apk add --no-cache \
    mariadb-client \
    vim && \
    docker-php-ext-install bcmath

# set recommended PHP.ini settings
# see https://secure.php.net/manual/en/opcache.installation.php
RUN { \
		echo 'opcache.memory_consumption=128'; \
		echo 'opcache.interned_strings_buffer=8'; \
		echo 'opcache.max_accelerated_files=4000'; \
		echo 'opcache.revalidate_freq=60'; \
		echo 'opcache.fast_shutdown=1'; \
	} > /usr/local/etc/php/conf.d/opcache-recommended.ini

################################Custom##########################################

ENV DRUSH_VERSION 8.3.0
RUN set -eux; \
        echo "memory_limit = 4096M"  >> ${php_vars} && \
        # drush command
        curl -fsSL -o /usr/local/bin/drush "https://github.com/drush-ops/drush/releases/download/$DRUSH_VERSION/drush.phar" && \
        chmod +x /usr/local/bin/drush

WORKDIR /var/www/html

# https://www.drupal.org/node/3060/release
ENV DRUPAL_VERSION 7.69
ENV DRUPAL_MD5 292290a2fb1f5fc919291dc3949cdf7c

RUN set -eux; \
	curl -fSL "https://ftp.drupal.org/files/projects/drupal-${DRUPAL_VERSION}.tar.gz" -o drupal.tar.gz; \
	echo "${DRUPAL_MD5} *drupal.tar.gz" | md5sum -c -; \
	tar -xz --strip-components=1 -f drupal.tar.gz; \
	rm drupal.tar.gz; \
	chown -R www-data:www-data sites modules themes

RUN rm -rf /start.sh && \
    rm -rf /etc/nginx/sites-available/default.conf && \
    rm -rf /etc/nginx/sites-enabled/default.conf

ADD config/scripts/start.sh /start.sh
# Drupal nginx config
ADD config/conf/drupal7-default.conf /etc/nginx/sites-available/default.conf
ADD config/conf/drupal7-general.conf /etc/nginx/sites-available/drupal7-general.conf
RUN set -eux; \
        ln -s /etc/nginx/sites-available/default.conf /etc/nginx/sites-enabled/default.conf && \
        ln -s /etc/nginx/sites-available/drupal7-general.conf /etc/nginx/sites-enabled/drupal7-general.conf && \
        chmod 755 /start.sh

WORKDIR "/var/www/html"