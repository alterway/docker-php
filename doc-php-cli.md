# Docker PHP-CLI

## Version available

- PHP 8.1 (docker tags: `8.1-cli`) - `docker pull alterway/php:8.1-cli`
- PHP 8.0 (docker tags: `8.0-cli`) - `docker pull alterway/php:8.0-cli`
- PHP 7.4 (docker tags: `7.4-cli`) - `docker pull alterway/php:7.4-cli`
- PHP 7.3 (docker tags: `7.3-cli`) - `docker pull alterway/php:7.3-cli` [DEPRECATED]
- PHP 7.2 (docker tags: `7.2-cli`) - `docker pull alterway/php:7.2-cli` [DEPRECATED]
- PHP 7.1 (docker tags: `7.1-cli`) - `docker pull alterway/php:7.1-cli` [DEPRECATED]
- PHP 7.0 (docker tags: `7.0-cli`) - `docker pull alterway/php:7.0-cli` [DEPRECATED]
- PHP 5.6 (docker tags: `5.6-cli`) - `docker pull alterway/php:5.6-cli` [DEPRECATED]
- PHP 5.5 (docker tags: `5.5-cli`) - `docker pull alterway/php:5.5-cli` [DEPRECATED]
- PHP 5.4 (docker tags: `5.4-cli`) - `docker pull alterway/php:5.4-cli` [DEPRECATED]


## Presentation

The entrypoint run `php` command line

The default workdir is `/var/www/` and the default Apache DocumentRoot path is `/var/www/html`.

Extra-packages available : composer, curl, wget, git, subversion, mysql-client

## Usage

    hub.alterway.fr/php:[tag] [options] [arguments]

### Example

show version :

    docker run --rm -it hub.alterway.fr/php:5.6-cli -v

run file script :

    docker run --rm -it --user $USER -v /etc/passwd:/etc/passwd:ro -v $PWD:$PWD -w=$PWD hub.alterway.fr/php:5.6-cli -f myscript.php


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
        PHP_php5enmod: 'memcached mysqli opcache'

#### Extensions available
- php >= 5.3 : `bcmath gd gmp intl ldap mbstring memcached mongo mysql mysqli pcntl pdo_mysql redis soap zip blackfire ftp sockets`
- php >= 5.4 : `xdebug`
- php >= 5.5 : `opcache`
- php >= 5.6 : `zlib`

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


## Use docker links [DEPRECATED]

### [Warning] Future versions of Docker will not support links - you should remove them for forwards-compatibility.

Set link with alias :

- `smtp` : set ssmtp configuration
- `php_memcached` : set php session.save_handler to memcached (use PHP_MEMCACHED_PORT_11211_TCP_ADDR and PHP_MEMCACHED_PORT_11211_TCP_PORT)

## Contributors

- [Nicolas Berthe](https://github.com/4devnull)

## License

View [LICENSE](https://github.com/alterway/docker-php/blob/master/LICENSE) for the software contained in this image.
