# Docker PHP - Apache

## Version available

- Apache/2.4 - PHP/5.6 (docker tags: `5.6-apache`) - `docker pull alterway/php:5.6-apache`
- Apache/2.4 - PHP/5.5 (docker tags: `5.5-apache`) - `docker pull alterway/php:5.5-apache`
- Apache/2.4 - PHP/5.4 (docker tags: `5.4-apache`) - `docker pull alterway/php:5.4-apache` [DEPRECATED]
- Apache/2.2 - PHP/5.3.29 (docker tags: `5.3-apache`) - `docker pull alterway/php:5.3-apache` [DEPRECATED]

## Presentation

The entrypoint run `apache2` daemon by default and expose port 80.

The default workdir is `/var/www/` and the default Apache DocumentRoot path is `/var/www/html`.


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


### Set your apache.conf

The apache configuration is dynamic. Just add environment variable with prefix `HTTPD__`.

Example with docker-compose :

    ...
    environment:
        HTTPD__DocumentRoot: '/var/www/public'
        HTTPD__ServerAdmin: 'webmaster@example.org'
        HTTPD__AddDefaultCharset: 'UTF-8'
        HTTPD__DirectoryIndex: 'app.php'

### Load Apache modules
 
The apache modules are load on start. Just add environment variable `HTTPD_a2enmod` with list of your modules

Example with docker-compose :

    environment:    
        HTTPD_a2enmod:  'rewrite status expires'
        
Modules available :

    access_compat actions alias allowmethods asis auth_basic auth_digest auth_form authn_anon authn_core authn_dbd authn_dbm authn_file authn_socache authnz_fcgi authnz_ldap authz_core authz_dbd authz_dbm authz_groupfile authz_host authz_owner authz_user autoindex buffer cache cache_disk cache_socache cgi cgid charset_lite data dav dav_fs dav_lock dbd deflate dialup dir dump_io echo env expires ext_filter file_cache filter headers heartbeat heartmonitor ident include info lbmethod_bybusyness lbmethod_byrequests lbmethod_bytraffic lbmethod_heartbeat ldap log_debug log_forensic lua macro mime mime_magic mpm_event mpm_prefork mpm_worker negotiation php5 proxy proxy_ajp proxy_balancer proxy_connect proxy_express proxy_fcgi proxy_fdpass proxy_ftp proxy_html proxy_http proxy_scgi proxy_wstunnel ratelimit reflector remoteip reqtimeout request rewrite sed session session_cookie session_crypto session_dbd setenvif slotmem_plain slotmem_shm socache_dbm socache_memcache socache_shmcb speling ssl status substitute suexec unique_id userdir usertrack vhost_alias xml2enc

Modules default enabled : 

    access_compat alias auth_basic authn_core authn_file authz_core authz_host authz_user autoindex deflate dir env filter mime mpm_prefork negotiation php5 rewrite setenvif status

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
- `php_memcached` : set php session.save_handler to memcached (use PHP_MEMCACHED_PORT_11211_TCP_ADDR and PHP_MEMCACHED_PORT_11211_TCP_PORT

## Contributors

- [Nicolas Berthe](https://github.com/4devnull)

## License

View [LICENSE](https://github.com/alterway/docker-php/blob/master/LICENSE) for the software contained in this image.