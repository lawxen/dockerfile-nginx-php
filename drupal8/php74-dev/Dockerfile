FROM lawxen/drupal:8-nginx-php74

# Install xdebug
RUN docker-php-ext-enable xdebug && \
    rm -rf /start.sh
ADD config/scripts-dev/start.sh /start.sh
RUN chmod 755 /start.sh