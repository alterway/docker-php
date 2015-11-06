# Docker PHP-FPM

## Version available

- PHP-FPM 5.6 (docker tags: `5.6-fpm`) - `docker pull hub.alterway.fr/php:5.6-fpm`
- PHP-FPM 5.4 (docker tags: `5.4-fpm`) - `docker pull hub.alterway.fr/php:5.4-fpm`

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

Extensions available : `bcmath gd gmp intl ldap mbstring mcrypt memcached mongo mysql mysqli pcntl pdo_mysql redis soap zip opcache`

## Use docker links

Set link with alias :

- `smtp` : set ssmtp configuration
- `php_memcached` : set php session.save_handler to memcached (use PHP_MEMCACHED_PORT_11211_TCP_ADDR and PHP_MEMCACHED_PORT_11211_TCP_PORT

## Contributors

- [Nicolas Berthe](https://github.com/4devnull)

## License

View [LICENSE](https://github.com/alterway/docker-php/blob/master/LICENSE) for the software contained in this image.