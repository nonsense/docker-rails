## PostgreSQL container

PGSQL=$(sudo docker run -p 5432:5432 -name pgsql -d project/pgsql /usr/bin/start_pgsql.sh YOURPASSWORD)
PGSQL_IP=$(sudo docker inspect $PGSQL | grep IPAddress | awk '{ print $2 }' | tr -d ',"')

## Connect through the client

psql -h $PGSQL_IP -Uroot -d postgres

## Redis container

REDIS=$(sudo docker run -p 6379:6379 -d -name redis project/redis)
REDIS_IP=$(sudo docker inspect $REDIS | grep IPAddress | awk '{ print $2 }' | tr -d ',"')

## Verify that containers are working

docker ps $PGSQL
docker ps $REDIS

## VMware Fusion folder mapping and container linking

sudo docker run -i -p 80:80 -name rails -v /mnt/hgfs/project/rails/src:/var/www/project -link pgsql:pgsql -link redis:redis -t project/rails /bin/bash

## VirtualBox / Vagrant folder mapping and container linking

sudo docker run -i -privileged -p 80:80 -name rails -v /vagrant/rails/src:/var/www/project -link pgsql:pgsql -link redis:redis -t project/rails /bin/bash


[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/nonsense/docker-rails/trend.png)](https://bitdeli.com/free "Bitdeli Badge")

