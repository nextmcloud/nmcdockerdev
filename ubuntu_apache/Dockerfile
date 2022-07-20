FROM --platform=linux/amd64 nextcloud:apache

# install common tools
# Install phpunit
# INstall php-cs-fixer
# Install krankerl
# Install Composer for krankerl (only)
# Install nodejs,npm for krankerl (only)
RUN apt-get update \
    && apt-get install -y jq supervisor cron sqlite3\
    && mkdir -p /var/log/supervisord \
    && mkdir -p /var/run/supervisord \
    && curl -L https://cs.symfony.com/download//php-cs-fixer-v3.phar -o php-cs-fixer > /usr/local/bin/php-cs-fixer \
    && chmod +x /usr/local/bin/php-cs-fixer \
    && curl -L https://phar.phpunit.de/phpunit-8.phar > /usr/local/bin/phpunit \
    && chmod +x /usr/local/bin/phpunit \
    && curl -L https://github.com/ChristophWurst/krankerl/releases/download/v0.13.1/krankerl > /usr/local/bin/krankerl \
    && chmod +x /usr/local/bin/krankerl \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && curl -sL https://deb.nodesource.com/setup_14.x | bash \
    && apt-get install -y nodejs \
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

# see https://github.com/nextcloud/docker/.examples
COPY supervisord.conf /etc

ENV NEXTCLOUD_UPDATE=0

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]


