FROM richarvey/nginx-php-fpm:1.9.1

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

RUN set -eux; \
        echo "memory_limit = 4096M"  >> ${php_vars} && \
        # Install Drush
        curl -fsSL -o /usr/local/bin/drush "https://github.com/drush-ops/drush-launcher/releases/download/$DRUSH_VERSION/drush.phar" && \
        chmod +x /usr/local/bin/drush && \
        # Install Drupal Console
        curl https://drupalconsole.com/installer -L -o drupal.phar && \
        mv drupal.phar /usr/local/bin/drupal && \
        chmod +x /usr/local/bin/drupal

WORKDIR "/var/www"

# Install Drupal
RUN set -eux; \
        rm -rf /var/www/html && \
        composer create-project drupal/recommended-project drupal8-download && \
        cd drupal8-download && \
        composer require drush/drush && \
        chown -R www-data:www-data web/sites web/modules web/themes

RUN set -eux; \
        rm -rf /start.sh && \
        rm -rf /etc/nginx/sites-available/default.conf && \
        rm -rf /etc/nginx/sites-enabled/default.conf

ADD config/scripts/start.sh /start.sh
# Drupal nginx config
ADD config/conf/drupal8-default.conf /etc/nginx/sites-available/default.conf
ADD config/conf/drupal8-general.conf /etc/nginx/sites-available/drupal8-general.conf
RUN set -eux; \
        ln -s /etc/nginx/sites-available/default.conf /etc/nginx/sites-enabled/default.conf && \
        ln -s /etc/nginx/sites-available/drupal8-general.conf /etc/nginx/sites-enabled/drupal8-general.conf && \
        chmod 755 /start.sh

WORKDIR "/var/www/html"