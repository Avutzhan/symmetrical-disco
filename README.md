 #Laradock 
 ##One project one laradock

* To understand what is Laradock [Laradock Habr Article](https://habr.com/ru/post/439346/)
* [This Tutorial](https://dev.to/moghwan/dockerize-your-laravel-project-with-laradock-2io1)
* [Laradock Documentation](https://laradock.io/documentation/)
* [PhpStorm doesn't see root directory](https://stackoverflow.com/questions/48065971/phpstorm-not-showing-project-files-in-project-view)

##Quick Start

1. Open /etc/hosts
```shell
# /etc/hosts (linux)
# C:\Windows\System32\drivers\etc\hosts (Windows)

127.0.0.1 laraveldock.test
```
2. Prepare Laravel Project
```shell
# You can use version 7 or 8
composer create-project --prefer-dist laravel/laravel laraveldock "7.*.*"

cd laraveldock
git init
git add .
git commit -m "init commit"
```

3. Installing and Configuring Laradock

```shell
# module directory should be unique for running multiple Laradock instances
git submodule add https://github.com/Laradock/laradock.git laradock-laraveldock

# go to module dir
cd laradock-laraveldock

# create a .env file from the example
cp env-example .env

# open docker compose config file to add our virtual host
code docker-compose.yml
```

```yaml
# approximately line 375
nginx:
    # - frontend
    # - backend
    networks:
        frontend:
          aliases:
            - laraveldock.test
        backend:
          aliases:
            - laraveldock.test
```
4. Building docker images

```shell
cd laradock-laraveldock

# for mysql
docker-compose up -d --build nginx mysql

# for posqtgresql
docker-compose up -d --build nginx postgres
```
5. Setting up database

```shell
docker-compose down

docker-compose up -d --build nginx phpmyadmin
```

laraveldock.test:8081

* Hostname: mysql
* Username: root
* Password: root

```shell
docker-compose exec mysql bash

  mysql -uroot -proot
  create database lara_db;
  exit

exit
```
```shell
docker-compose exec workspace bash
artisan migrate
```

* Enter to containers

```shell
docker-compose up -d --build nginx mysql workspace php-fpm (this is a name of containers)
```

```shell
docker-compose exec workspace bash
docker-compose exec php-fpm bash
```

