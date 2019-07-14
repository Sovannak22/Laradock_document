# Install laradock with php project.

In this document I'll build project with apache2 mysql container. <br>
You can also check Sopported software (Image) by visit this link https://laradock.io/introduction/

## Project setup
* Clone laradock from https://github.com/Laradock/laradock.git
    * If you arlready have php project (git submodule add https://github.com/Laradock/laradock.git)
    * If you don't have php project yet (git clone https://github.com/Laradock/laradock.git)
## Project configuration
* Enter laradock folder (`cd laradock`) Note:If you have many laravel project rename your laradock folder 
* Copy `env-example` `.env`
```
    cp env-example .env
```
* Make environment for your application (Ex: apache mysql ..) Note: make sure the configuration is the same as `env` variable in `.env` file in project .

<br>

### Configuration for mysql

**Inside laradock `.env` file**
```
    ### MYSQL #################################################

    MYSQL_VERSION=latest
    MYSQL_DATABASE=db
    MYSQL_USER=userdb
    MYSQL_PASSWORD=12345
    MYSQL_PORT=3306
    MYSQL_ROOT_PASSWORD=root
    MYSQL_ENTRYPOINT_INITDB=./mysql/docker-entrypoint-initdb.d
```
<br>

**Inside project `.env` file**
<br>

```
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=db
DB_USERNAME=userdb
DB_PASSWORD=12345

```

### Configuration for apache
You can change default port for your apach2 container by :

```
### APACHE ################################################

APACHE_HOST_HTTP_PORT=8081
APACHE_HOST_HTTPS_PORT=443
APACHE_HOST_LOG_PATH=./logs/apache2
APACHE_SITES_PATH=./apache2/sites
APACHE_PHP_UPSTREAM_CONTAINER=php-fpm
APACHE_PHP_UPSTREAM_PORT=9000
APACHE_PHP_UPSTREAM_TIMEOUT=60
APACHE_DOCUMENT_ROOT=/var/www/
```

## Build environment and run project

This is the combination of **apach2** **mysql** if you want to run your own combination you can visit this link `https://laradock.io/getting-started/#usage` . 
```
    docker-compose up -d apache2 php-fpm mysql
```
## Enter work space to execute command for project
```
    docker-compose exec workspace bash
```
By this command you can go to container project directory and execute like (`php artisan migrate`,`composer install` `...`)


## Error
If you getting error with updating database try to remove old database by comment 
```
    rm -rf ~/.laradock/data/mysql
```
### SQLSTATE[HY000] [2054] The server requested authentication method unknown to the client
**1. add default authentication plugin to laradock/mysql/my.cnf**
```
[mysqld]
default_authentication_plugin= mysql_native_password
```
**2. update content file mysql/docker-entrypoint-initdb.d/createdb.sql like:**
```
CREATE USER 'admin'@'localhost' IDENTIFIED WITH mysql_native_password BY 'yourpass';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;
CREATE USER 'admin'@'%' IDENTIFIED WITH mysql_native_password BY 'yourpass';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION;
#
CREATE DATABASE IF NOT EXISTS `yourdb` COLLATE 'utf8_general_ci' ;
GRANT ALL ON `yourdb`.* TO 'admin'@'%' ;
FLUSH PRIVILEGES ;
```
**3. Remove mysql container and re-run it.**
**4. php artisan migrate is done :)**
Some solution link for docker problem
* https://github.com/laradock/laradock/issues/1392
