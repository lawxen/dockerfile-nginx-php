FROM lawxen/drupal:7-nginx-php72

# Install xdebug
RUN docker-php-ext-enable xdebug && \
    rm -rf /start.sh
ADD config/scripts-dev/start.sh /start.sh
RUN chmod 755 /start.sh