Minimalistic PHP FPM container that listens on a unix socket - /var/run/docker/php.sock.

Could be useful paired with various web servers to avoid TCP/IP overhead between the web server and the fpm.

Here's an example docker-compose.yml file using rwasa:

```
version: '3.6'
services:
  php:
    image: dinamic/php-unix-socket:7.2
    volumes:
      - "/path/to/src:/var/www:cached"
      - "unixSocket:/var/run/docker:cached"
  rwasa:
    image: scttmthsn/rwasa
    volumes:
      - "/path/to/src:/var/www:cached"
      - "unixSocket:/var/run/docker:cached"
    ports:
      - 80:8080
    entrypoint:
      /rwasa -bind 8080 -foreground -sandbox /var/www/public -fastcgi .php /var/run/docker/php.sock -indexfiles index.html,index.php
volumes:
  unixSocket:
```
