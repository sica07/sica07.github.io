# Xdebug
[<TIL](Programming.md)

## Full content of var_dump/xdebug
```
ini_set("xdebug.var_display_max_children", -1);
ini_set("xdebug.var_display_max_data", -1);
ini_set("xdebug.var_display_max_depth", -1);
```
## Xdebug and Docker

Here is the relevant excerpt of the Dockerfile that installs Xdebug:

```
ARG WITH_XDEBUG=false

RUN if [ $WITH_XDEBUG = "true" ] ; then \
        pecl install xdebug; \
        docker-php-ext-enable xdebug; \
        echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
    fi ;
```

I dont want have to have a separate Dockerfile for development and production, so I have defined a build argument that will tell whether we want to install Xdebug or not.

Then, on my Docker-compose file I have the following definition for my application:

```

version: "3"

services:

  php:
    build:
      context: .
      args:
        - WITH_XDEBUG=true
    env_file: .env
    volumes:
      - .:/var/www/app:rw
```

We will define the following environment variables:

**PHP_IDE_CONFIG** - This variable defines the server configuration associated with the application.

**XDEBUG_CONFIG** - This variable allows to define some Xdebug configurations. The "remote host" is the private ip of your host machine (the one your PHPStorm is running). The "remote_port" is the port that PHPStorm will be listening for incoming Xdebug connections. These two settings allow PHPStorm and Xdebug to communicate. It wont work without this.

We will add them to our _.env_ file like this:
```
PHP_IDE_CONFIG=serverName=symfony-demo
XDEBUG_CONFIG=remote_host=192.168.1.102 remote_port=9001
```
[source](https://dev.to/brpaz/docker-phpstorm-and-xdebug-the-definitive-guide-14og)
