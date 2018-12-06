# laravel-alpine
Laravel PHP framework running on PHP-FPM with alpine base Docker Image 🐳

[![Docker Automated build](https://img.shields.io/docker/automated/kamerk22/laravel-alpine.svg?style=flat-square)](https://hub.docker.com/r/kamerk22/laravel-alpine)
[![Docker Stars](https://img.shields.io/docker/stars/kamerk22/laravel-alpine.svg?style=flat-square)](https://hub.docker.com/r/kamerk22/laravel-alpine)
[![Docker Pulls](https://img.shields.io/docker/pulls/kamerk22/laravel-alpine.svg?style=flat-square)](https://hub.docker.com/r/kamerk22/laravel-alpine)
[![GitHub](https://img.shields.io/github/license/mashape/apistatus.svg?style=flat-square)](https://github.com/kamerk22/laravel-alpine/)

![SIZE](http://i.imgur.com/oJ4jCPP.jpg)

## Pull it from Docker Registry
To pull the docker image:
```bash
docker pull kamerk22/laravel-alpine:latest
```

## Usage
To run from current dir
```bash
docker run -v $(pwd):/var/www kamerk22/laravel-alpine:latest "composer install --prefer-dist"
```

## What's Included
 - [Composer](https://getcomposer.org/) ( with non Root user )
 - CRON ( pre-installed and configured to work with Laravel Scheduler )
 - [Supervisor](http://supervisord.org) 
 
## Docker hub tags
You can use following tags on Docker hub:
- `latest` - Default
- `mysql` - MySQL driver 
- `pgsql` - PostgreSQL driver

## Other Details
- PHP - 7.2
- Alpine - 3.8

## PHP Extension
- opcache
- mysqli
- pgsql 
- pdo 
- pdo_mysql
- pdo_pgsql 
- sockets
- json
- intl
- gd
- xml
- zip
- bz2
- pcntl
- bcmath

## Adding other PHP Extension
You can add additional PHP Extensions by running `docker-ext-install` command. Don't forget to install necessary dependencies for required extension.
```bash
FROM kamerk22/laravel-alpine:latest
RUN docker-php-ext-install memcached
```

 ## Adding custom CRON
 ```bash
 FROM kamerk22/laravel-alpine:latest
 echo '0 * * ? * * /usr/local/bin/php  /var/www/artisan schedule:run >> /dev/null 2>&1' > /etc/crontabs/root 
 ```
 
 ## Adding custom Supervisor config
 You can add your own Supervisor config inside `/etc/supervisor.d/` for Laravel Queue or Laravel Horizon. File extension needs to be `*.ini`. By default this image added `php-fpm` and `crond` process in supervisor. 

E.g: For Laravel Horizon make file `horizon.ini`
```ini
[program:horizon]
process_name=%(program_name)s
command=php /home/forge/app.com/artisan horizon
autostart=true
autorestart=true
user=forge
redirect_stderr=true
stdout_logfile=/home/forge/app.com/horizon.log
```
On your Docker image
```bash
FROM kamerk22/laravel-alpine:latest
ADD horizon.ini /etc/supervisor.d/
```
For more details on config http://supervisord.org/configuration.html

## Troubleshooting / Issues / Contributing
Feel free to open an issue in this repository or contact me on [Twitter](https://twitter.com/kamerk22).


