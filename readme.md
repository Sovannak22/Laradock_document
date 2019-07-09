#install laradock with php project

* Clone laradock from https://github.com/Laradock/laradock.git
    * If you arlready have php project (git submodule add https://github.com/Laradock/laradock.git)
    * If you don't have php project yet (git clone https://github.com/Laradock/laradock.git)
* Enter laradock folder (cd laradock) Note:If you have many laravel project rename your laradock folder 
* Copy env-example .env
* Set up environment for your application (Ex: apache mysql ..) Note: make sure the configuration is the same as env variable in .env file in project
* Build the environment and run it

If you getting error with updating database try to remove old database by comment 
```
    rm -rf ~/.laradock/data/mysql
```
