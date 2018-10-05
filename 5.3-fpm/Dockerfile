FROM debian:wheezy
LABEL maintainer="Alterway <iac@alterway.fr>"

RUN apt-get update && \
    apt-get install -y ca-certificates curl libpcre3 librecode0 libsqlite3-0 libxml2 autoconf file g++ gcc libc-dev make pkg-config re2c --no-install-recommends && \
    rm -r /var/lib/apt/lists/*

ENV PHP_INI_DIR /usr/local/etc/php
RUN mkdir -p $PHP_INI_DIR/conf.d

ENV PHP_EXTRA_CONFIGURE_ARGS --enable-fpm --with-fpm-user=www-data --with-fpm-group=www-data

ENV GPG_KEYS 0A95E9A026542D53835E3F3A7DEC4E69FC9C83D7
RUN set -xe \
	&& for key in $GPG_KEYS; do \
		gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
	done

ENV PHP_VERSION 5.3.29

RUN buildDeps=" \
		$PHP_EXTRA_BUILD_DEPS \
		bzip2 \
		libcurl4-openssl-dev \
		libpcre3-dev \
		libreadline6-dev \
		librecode-dev \
		libsqlite3-dev \
		libssl-dev \
		libxml2-dev \
	" \
	&& set -x \
	&& apt-get update && apt-get install -y $buildDeps --no-install-recommends && rm -rf /var/lib/apt/lists/* \
	&& curl -SL "http://php.net/get/php-$PHP_VERSION.tar.bz2/from/this/mirror" -o php.tar.bz2 \
	&& curl -SL "http://php.net/get/php-$PHP_VERSION.tar.bz2.asc/from/this/mirror" -o php.tar.bz2.asc \
	&& gpg --verify php.tar.bz2.asc \
	&& mkdir -p /usr/src/php \
	&& tar -xof php.tar.bz2 -C /usr/src/php --strip-components=1 \
	&& rm php.tar.bz2*

RUN	cd /usr/src/php \
	&& ./configure \
		--with-config-file-path="$PHP_INI_DIR" \
		--with-config-file-scan-dir="$PHP_INI_DIR/conf.d" \
		$PHP_EXTRA_CONFIGURE_ARGS \
		--disable-cgi \
		--enable-mysqlnd \
		--with-curl \
		--with-openssl \
		--with-pcre \
		--with-readline \
		--with-recode \
		--with-zlib \
	&& make -j"$(nproc)" \
	&& make install \
	&& { find /usr/local/bin /usr/local/sbin -type f -executable -exec strip --strip-all '{}' + || true; } \
	&& apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false -o APT::AutoRemove::SuggestsImportant=false $buildDeps \
	&& make clean

COPY docker-php-ext-* /usr/local/bin/

WORKDIR /var/www/html
COPY php-fpm.conf /usr/local/etc/

EXPOSE 9000
CMD ["php-fpm"]

RUN apt-get update && \
    apt-get install -y \
        libfreetype6-dev \
        libmcrypt-dev \
        libpng12-dev \
        libjpeg-dev \
        libgmp-dev \
        libxml2-dev \
        zlib1g-dev \
        libncurses5-dev \
        libldap2-dev \
        libicu-dev \
        libmemcached-dev \
        libcurl4-openssl-dev \
        libssl-dev \
        php-pear \
        curl \
        ssmtp \
        mysql-client \
        git \
        subversion \
        wget && \
    rm -rf /var/lib/apt/lists/* && \
    wget https://getcomposer.org/download/1.2.4/composer.phar -O /usr/local/bin/composer && \
    chmod a+rx /usr/local/bin/composer

RUN docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd && \
    docker-php-ext-configure mysql --with-mysql=mysqlnd && \
    docker-php-ext-configure mysqli --with-mysqli=mysqlnd && \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/lib && \
    docker-php-ext-install pdo_mysql && \
    docker-php-ext-install mysqli && \
    docker-php-ext-install mysql && \
    docker-php-ext-install ldap && \
    docker-php-ext-install soap && \
    docker-php-ext-install intl && \
    docker-php-ext-install mcrypt && \
    docker-php-ext-install gd && \
    docker-php-ext-install gmp && \
    docker-php-ext-install bcmath && \
    docker-php-ext-install mbstring && \
    docker-php-ext-install zip && \
    docker-php-ext-install pcntl && \
    docker-php-ext-install ftp && \
    docker-php-ext-install sockets && \
    pecl install mongo && \
    pecl install memcached-1.0.2 && \
    pecl install redis

ENV LOCALTIME Europe/Paris
ENV PHPFPM__access.format '"%R - %u [%t] \"%m %r\" %s %l %Q %f"'


RUN rm $PHP_INI_DIR/conf.d/docker-php-ext* && \
    echo 'sendmail_path = /usr/sbin/ssmtp -t' >> $PHP_INI_DIR/conf.d/00-default.ini && \
    echo "\ninclude=/usr/local/etc/php-fpm.d/*.conf" >> /usr/local/etc/php-fpm.conf && \
    mkdir -p /usr/local/etc/php-fpm.d && \
    chmod a+w -R $PHP_INI_DIR/conf.d/ /etc/ssmtp /usr/local/etc/php-fpm.d

ADD https://blackfire.io/api/v1/releases/probe/php/linux/amd64/53 /tmp/blackfire-probe.tar.gz
RUN tar zxpf /tmp/blackfire-probe.tar.gz -C /tmp && \
    mv /tmp/blackfire-*.so `php -r "echo ini_get('extension_dir');"`/blackfire.so && \
    rm /tmp/blackfire-probe.tar.gz

COPY docker-entrypoint.sh /entrypoint.sh

WORKDIR /var/www

ENTRYPOINT ["/entrypoint.sh"]
CMD ["php-fpm"]