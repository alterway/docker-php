# Docker PHP-FPM

## Version available

- PHP-FPM 5.6 (docker tags: `5.6-fpm`) - `docker pull alterway/php:5.6-fpm`
- PHP-FPM 5.6 with extra-packages (docker tags: `5.6-fpm-extra`) - `docker pull alterway/php:5.6-fpm-extra`
- PHP-FPM 5.5 (docker tags: `5.5-fpm`) - `docker pull alterway/php:5.5-fpm`
- PHP-FPM 5.4 (docker tags: `5.4-fpm`) - `docker pull alterway/php:5.4-fpm` [DEPRECATED]
- PHP-FPM 5.3.29 (docker tags: `5.3-fpm`) - `docker pull alterway/php:5.3-fpm` [DEPRECATED]

## Presentation

The default workdir is `/var/www/`.

The entrypoint start php-fpm and expose port `9000`

## Environment variables

### Set your php.ini

The php configuration is dynamic. Just add environment variable with prefix `PHP__`.

Example with docker-compose :

    ...
    environment:
        PHP__display_errors: 'On'
        PHP__opcache.enable: 'Off'
        PHP__memory_limit:   '128M'
        PHP__post_max_size:  '50M'
        PHP__date.timezone:  '"Europe/Paris"'
        
### Load PHP Extensions

The PHP Extensions are load on start. Just add environment variable `PHP_php5enmod` with list of your extensions

Example with docker-compose :

    ...
    environment:
        PHP_php5enmod: 'mcrypt memcached mysqli opcache'

#### Extensions available
- php >= 5.3 : `bcmath gd gmp intl ldap mbstring mcrypt memcached mongo mysql mysqli pcntl pdo_mysql redis soap zip blackfire`
- php >= 5.4 : `xdebug`
- php >= 5.5 : `opcache`

### Set your php-fpm.conf

The php-fpm configuration is dynamic. Just add environment variable with prefix `PHPFPM__`.

Example with docker-compose :
    
     environment:
        PHPFPM__pm: dynamic
        PHPFPM__pm.max_children: 5
        PHPFPM__pm.start_servers: 2
        PHPFPM__pm.min_spare_servers: 1
        PHPFPM__pm.max_spare_servers: 3


### Advanced Environment variables

- `MEMCACHED` : Enable session.save_handler to memcached and set address list of memcached (Format `address:port address:port ...`)
- `MEMCACHED_CONFIG`: Set options of memcached (default: `persistent=1&timeout=5&retry_interval=30`)
- `SMTP` : set address of mail server (Format `address:port`)
- `PHP__blackfire.agent_socket` : set address of blackfire agent (Format `tcp://address:port`)

Example with docker-compose :

    ...
    environment:  
       MEMCACHED:                      'memcached_1:11211 memcached_2:11211'
       MEMCACHED_CONFIG:               'timeout=5&retry_interval=60'
       SMTP:                           'mailcatcher_1:25'
       PHP__blackfire.agent_socket:    'tcp://blackfire:8707'


## Use docker links

### [DEPRECATED] Future versions of Docker will not support links - you should remove them for forwards-compatibility.

Set link with alias :

- `smtp` : set ssmtp configuration
- `php_memcached` : set php session.save_handler to memcached (use PHP_MEMCACHED_PORT_11211_TCP_ADDR and PHP_MEMCACHED_PORT_11211_TCP_PORT)

## PHP-FPM 5.6 with Extra-packages

This version is extend to `php:5.6-fpm`

Available packages : composer.phar (1.0.0-alpha11), curl, wget, git, subversion, mysql-client

## Contributors

- [Nicolas Berthe](https://github.com/4devnull)

## License

View [LICENSE](https://github.com/alterway/docker-php/blob/master/LICENSE) for the software contained in this image.