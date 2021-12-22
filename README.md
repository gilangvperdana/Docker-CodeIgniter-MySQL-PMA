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
