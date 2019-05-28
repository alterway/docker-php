FROM php:7.1-alpine
LABEL maintainer="Alterway <iac@alterway.fr>"

RUN echo "http://dl-cdn.alpinelinux.org/alpine/v3.6/community" >> /etc/apk/repositories && \
    echo "http://dl-cdn.alpinelinux.org/alpine/v3.6/main" >> /etc/apk/repositories && \
    apk add --update --no-cache \
        git \
        subversion \
        curl \
        wget \
        ssmtp \
        mysql-client \
        mariadb-dev \
        openldap-dev \
        libldap \
        libssl1.0 \
        libsasl \
        freetype-dev \
        libjpeg-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libxml2-dev \
        zlib-dev \
        php7-sqlite3 \
        php7-pdo_sqlite \
        sqlite-dev \
        php7-gmp \
        gmp-dev \
        php7-pear \
        ncurses-dev \
        icu-dev \
        libmemcached-dev \
        libcurl && \
    wget https://getcomposer.org/download/1.8.5/composer.phar -O /usr/local/bin/composer && \
    chmod a+rx /usr/local/bin/composer

RUN docker-php-ext-configure ldap && \
    docker-php-ext-install ldap && \
    docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd && \
    docker-php-ext-install pdo_mysql && \
    docker-php-ext-configure mysqli --with-mysqli=mysqlnd && \
    docker-php-ext-install mysqli && \
    docker-php-ext-install pdo_sqlite && \
    docker-php-ext-configure gd && \
    docker-php-ext-install gd && \
    docker-php-ext-install soap && \
    docker-php-ext-install intl && \
    docker-php-ext-install mcrypt && \
    docker-php-ext-install bcmath && \
    docker-php-ext-install mbstring && \
    docker-php-ext-install zip && \
    docker-php-ext-install pcntl && \
    docker-php-ext-install ftp && \
    docker-php-ext-install sockets && \
    docker-php-ext-install gmp

ADD http://www.zlib.net/zlib-1.2.11.tar.gz /tmp/zlib.tar.gz
ADD https://blackfire.io/api/v1/releases/probe/php/linux/amd64/71 /tmp/blackfire-probe.tar.gz
RUN apk add --update --no-cache tar gcc libc-dev make && \
    tar zxpf /tmp/zlib.tar.gz -C /tmp && \
    cd /tmp/zlib-1.2.11 && \
    ./configure --prefix=/usr/local/zlib && \
    make && make install && \
    rm -Rf /tmp/zlib-1.2.11 && \
    rm /tmp/zlib.tar.gz && \
    tar zxpf /tmp/blackfire-probe.tar.gz -C /tmp && \
    mv /tmp/blackfire-*.so `php -r "echo ini_get('extension_dir');"`/blackfire.so && \
    rm /tmp/blackfire-probe.tar.gz && \
    apk del --purge tar gcc libc-dev make

ENV LOCALTIME Europe/Paris

RUN rm $PHP_INI_DIR/conf.d/docker-php-ext* && \
    echo 'sendmail_path = /usr/sbin/ssmtp -t' >> $PHP_INI_DIR/conf.d/00-default.ini && \
    chmod a+w -R $PHP_INI_DIR/conf.d/ /etc/ssmtp

COPY docker-entrypoint.sh /entrypoint.sh

WORKDIR /var/www

ENTRYPOINT ["/entrypoint.sh"]
