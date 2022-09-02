FROM php:8.0-cli
LABEL maintainer="Alterway <iac@alterway.fr>"

RUN apt-get update && \
    apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libgmp-dev \
        libxml2-dev \
        zlib1g-dev \
        libncurses5-dev \
        libldb-dev \
        libldap2-dev \
        libicu-dev \
        libmemcached-dev \
        libcurl4-openssl-dev \
        libssl-dev \
        libsqlite3-dev \
        libzip-dev \
        libonig-dev \
        curl \
        msmtp \
        mariadb-client \
        git \
        subversion \
        wget && \
    rm -rf /var/lib/apt/lists/* && \
    wget https://getcomposer.org/download/1.10.17/composer.phar -O /usr/local/bin/composer && \
    wget https://getcomposer.org/download/2.0.7/composer.phar -O /usr/local/bin/composer2 && \
    chmod a+rx /usr/local/bin/composer /usr/local/bin/composer2

RUN ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h && \
    ln -s /usr/lib/x86_64-linux-gnu/libldap.so /usr/lib/libldap.so && \
    ln -s /usr/lib/x86_64-linux-gnu/liblber.so /usr/lib/liblber.so && \
    docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-install ldap && \
    docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd && \
    docker-php-ext-install pdo_mysql && \
    docker-php-ext-configure mysqli --with-mysqli=mysqlnd && \
    docker-php-ext-install mysqli && \
    docker-php-ext-install pdo_sqlite && \
    docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-install gd && \
    docker-php-ext-install soap && \
    docker-php-ext-install intl && \
    docker-php-ext-install gmp && \
    docker-php-ext-install bcmath && \
    docker-php-ext-install mbstring && \
    docker-php-ext-install zip && \
    docker-php-ext-install pcntl && \
    docker-php-ext-install ftp && \
    docker-php-ext-install sockets && \
    pecl install mongodb && \
    pecl install memcached && \
    pecl install redis && \
    pecl install xdebug

ADD http://www.zlib.net/zlib-1.2.12.tar.gz /tmp/zlib.tar.gz
RUN tar zxpf /tmp/zlib.tar.gz -C /tmp && \
    cd /tmp/zlib-1.2.12 && \
    ./configure --prefix=/usr/local/zlib && \
    make && make install && \
    rm -Rf /tmp/zlib-1.2.12 && \
    rm /tmp/zlib.tar.gz

ADD https://blackfire.io/api/v1/releases/probe/php/linux/amd64/80 /tmp/blackfire-probe.tar.gz
RUN tar zxpf /tmp/blackfire-probe.tar.gz -C /tmp && \
    mv /tmp/blackfire-*.so `php -r "echo ini_get('extension_dir');"`/blackfire.so && \
    rm /tmp/blackfire-probe.tar.gz

ENV LOCALTIME Europe/Paris

RUN rm $PHP_INI_DIR/conf.d/docker-php-ext* && \
    echo 'sendmail_path = /usr/bin/msmtp -t' >> $PHP_INI_DIR/conf.d/00-default.ini && \
    touch /etc/msmtprc && chmod a+w -R $PHP_INI_DIR/conf.d/ /etc/msmtprc

COPY docker-entrypoint.sh /entrypoint.sh

WORKDIR /var/www

ENTRYPOINT ["/entrypoint.sh"]
