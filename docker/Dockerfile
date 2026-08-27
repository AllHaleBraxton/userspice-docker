# Use the official PHP image with Apache
FROM php:8.4-apache

# Install MySQL extension and other dependencies
RUN docker-php-ext-install mysqli pdo pdo_mysql

# Install wget and unzip for downloading UserSpice
RUN apt-get update && apt-get install -y \ 
    wget \ 
    libzip-dev \ 
    a2enmod remoteip \
    && echo '<IfModule mod_remoteip.c>\n\
    RemoteIPHeader X-Forwarded-For\n\
    RemoteIPInternalProxy 199.1.1.100\n\
</IfModule>' > /etc/apache2/conf-available/remoteip.conf \
    && a2enconf remoteip \
    unzip \ 
    && docker-php-ext-install zip \ 
    && rm -rf /var/lib/apt/lists/*

# Enable mod_rewrite for Apache
RUN a2enmod rewrite

# Copy Docker-configured init.php
COPY ./docker/init.php /docker/init.php

# Copy custom files overlay
COPY ./custom_files /custom_files

# Copy entrypoint script and deal with Windows line endings
COPY ./docker/entrypoint.sh /usr/local/bin/entrypoint.sh
RUN sed -i 's/\r$//' /usr/local/bin/entrypoint.sh && chmod +x /usr/local/bin/entrypoint.sh

# Expose port 80
EXPOSE 80

ENTRYPOINT ["entrypoint.sh"]
