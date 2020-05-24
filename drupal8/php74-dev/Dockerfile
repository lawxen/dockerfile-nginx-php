FROM lawxen/drupal:8-nginx-php74

# Install xdebug
RUN docker-php-ext-enable xdebug

# Xdebug config
# ENV XDEBUG_REMOTE_ENABLE 1
# ENV XDEBUG_MAX_NESTING_LEVEL 200
# ENV XDEBUG_REMOTE_HOST host.docker.internal
# ENV XDEBUG_REMOTE_PORT 9001
# ENV XDEBUG_REMOTE_AUTOSTART on
# ENV XDEBUG_IDEKEY PHPSTORM
RUN { \
    echo '[xdebug]'; \
    echo 'xdebug.remote_enable=1'; \
    echo 'xdebug.remote_handler=dbgp'; \
    echo 'xdebug.remote_host=host.docker.internal'; \
    echo 'xdebug.remote_port=9001'; \
    echo 'xdebug.remote_autostart=1'; \
    echo 'xdebug.remote_connect_back=1'; \
    echo 'xdebug.idekey=PHPSTORM'; \
    } >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini