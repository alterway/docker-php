FROM hub.alterway.fr/php:5.6-fpm

RUN apt-get update && \
    apt-get install -y \
        mysql-client \
        git \
        subversion \
        curl \
        wget && \
    rm -rf /var/lib/apt/lists/* && \
    wget https://getcomposer.org/download/1.0.0-alpha11/composer.phar -O /usr/local/bin/composer && \
    chmod a+rx /usr/local/bin/composer