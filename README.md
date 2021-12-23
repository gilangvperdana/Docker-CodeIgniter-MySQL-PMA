# CodeIgniter + MySQL + PMA on Docker
   Simply deploy CI-App with Docker.
   
# Environment:
```
1. Linux
2. Docker ($ apt install -y docker.io)
3. Docker Compose ($ apt install -y docker-compose)
```

# Deploy with Docker:
```
Deploy with SSL Connection:
$ cd Docker-CodeIgniter-MySQL-PMA/
$ chmod 777 -R public/
$ cp -r ../your_project/* public/
$ docker-compose up -d
$ docker ps (see container nginx id)
$ docker exec -it id_nginx_container bash
$ certbot --nginx -d domain.com -d www.domain.com

Access on :
https://your_ip for CI
http://your_ip:8282 for phpmyAdmin
```

# Database details:
```
Port mysql : 3306
mysql & phpmyadmin account:
check on .env file for change, but the default is:
username : ci
password : ci
```

# Change PHP Version :
```
If you want to change PHP version, just change the image on Dockerfile on directory public.
Example, if you want to change to version 7.2.34 you can change to FROM php:7.2.34-fpm .

After you change, just :
$ docker-compose build --no-cache php
$ docker-compose up -d
```

# Alternate Way :
```
CODEIGNITER HTTPS DEPLOY ON DOCKER.

Alternate way to deploy with :
Reverse Proxy for Https Deploy [https://github.com/gilangvperdana/Multiple-Https-Nginx-withOneServer]
Database Instance for MySql Instance [https://github.com/gilangvperdana/Docker-MySQL-PMA]
You can follow this method :

$ nano docker.compose.yml
---
version: '3'
services:

  apache:
    image: php:7.2-apache
    restart: always
    hostname: apache
    networks:
      default:
      sql_net:
    volumes:
      - ./app:/var/www/html #Make a "app" directory for CodeIgniter Project. Please make sure the environment of BASE_URL & Database configuration has been configured first.
    environment: #Optional
      - VIRTUAL_HOST=your.domain.com
      - VIRTUAL_PORT=80
      - LETSENCRYPT_HOST=your.domain.com
      - LETSENCRYPT_EMAIL=your@email.com
    expose:
      - 80

networks:
  default:
    external:
      name: nginx-proxy
  sql_net:
    external:
      name: database_sql_net
---
$ docker-compose up -d

Install some PHP Dependencies :
GOES TO INSIDE CONTAINER :
$ docker exec -it container_id bash
$ docker-php-ext-install mysqli #if you want to deploy with MySQL for Databases.
$ a2enmod rewrite
$ service apache2 restart

Make .htaccess on "app" directory CODEIGNITER Project :
---
RewriteEngine On
  RewriteBase /
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteCond $1 !^(templates|plugins)
  RewriteRule ^(.*)$ index.php/$1 [L]
---

  Access on https://your.domain.com
```
