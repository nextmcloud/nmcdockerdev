FROM nextcloud:apache

# do os update first
RUN apt-get update

# Install phpunit
RUN curl -L https://phar.phpunit.de/phpunit-8.phar > /usr/local/bin/phpunit \
    && chmod +x /usr/local/bin/phpunit

# Install krankerl
RUN curl -L https://github.com/ChristophWurst/krankerl/releases/download/v0.13.1/krankerl > /usr/local/bin/krankerl \
    && chmod +x /usr/local/bin/krankerl

# Install Composer for krankerl (only)
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install nodejs,npm for krankerl (only)
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash \
    && apt-get install nodejs \
    && npm install -g npm@7


# example for additional php packages
RUN set -ex; \
    \
    savedAptMark="$(apt-mark showmanual)"; \
    \
    apt-get update; \
    apt-get install -y --no-install-recommends \
       libxml2-dev \
    ; \
    \
    docker-php-ext-install \
        soap \
    ;

#    docker-php-ext-configure imap --with-kerberos --with-imap-ssl; \
#        bz2 \
#        imap \
#    ; \
#    pecl install smbclient; \
#    docker-php-ext-enable smbclient;