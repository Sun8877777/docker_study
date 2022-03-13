# docker_study
command:
docker container run -it --name <NAME> <IMAGE>:<TAG>
-
docker image ls
docker image pull
docker image inspect
docker container run --name buzbuz busybox
docker container ls -a
docker container run -P -d nginx
docker container ps
docker container inspect e8243dc2ec3f
docker container top e8243dc2ec3f
docker container attach e8243dc2ec3f
docker container logs e8243dc2ec3f
docker container stats e8243dc2ec3f
docker container exec -it  e8243dc2ec3f /bin/bash
docker container pause festive_bouman
docker container unpause festive_bouman
docker container rm -f -v buzbuz
docker container prune - все остановленные
------------------------------------------------
Запуск контейнеров
------------------------------------------------
docker container run --name buzbuz busybox
docker container run --rm --name buzbuz busybox
docker container run  -d nginx
docker container prune -f
docker container run -d --expose 3000 nginx
docker container run -d --expose 3000 -p 80:3000 nginx
docker container run -d --expose 3000 -p 8080:80 nginx
docker container run -d -p 8080:80/tcp -p 8081:80/udp nginx
docker container port 4083c2a3e97b86c8ae62c85d54e2cfb2567264f5056981a9209bc024efcd0efc
docker container exec -it  e624e539f951a102b5a4fe550f9d0fb859a2fe1d53a8f9c6d3862f07f45ce4a6 ls /usr/share/nginx/html
docker container run --name  wheather-app -d -p 80:3000 linuxacademycontent/weather-app
docker container logs <NAME>
----------
NGINX-EXAMPLE
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
  && ln -sf /dev/stderr /var/log/nginx/error.log
----------
docker container run -d --name ghost_blog \
-e database__client=mysql \
-e database__connection__host=mysql \
-e database__connection__user=root \
-e database__connection__password=P4sSw0rd0! \
-e database__connection__database=ghost \
-p 8080:2368 \
ghost:1-alpine
--------------------
Сети в docker
docker network ls
docker network inspect
docker network create
docker network create br00
docker network rm br00
docker network prune
docker container run -d --name network-test01 -p 8081:80 nginx
docker network create br01
docker network connect br01 network-test01
docker network disconnect br01 network-test01
docker network create --subnet <SUBNET> --gateway <GATEWAY> <NAME>
docker network create --subnet <SUBNET> --gateway <GATEWAY> \
--ip-range=<IP_RANGE> --driver=<DRIVER> <NAME>
docker network create --subnet 10.1.0.0/24 --gateway 10.1.0.1 br02
docker network create --subnet 10.1.0.0/16 --gateway 10.1.0.1  \
--ip-range=10.1.4.0/24 --driver=bridge --label=host4network br04
docker container run --name network-test-1 -it --network br04 centos /bin/bash
docker container run -d --name network-test02 --ip 10.1.4.102 --network br04 nginx
docker network create -d bridge --internal localhost
-------------
docker container run -d --name test_mysql \
-e MYSQL_ROOT_PASSWORD=P4sSw0rd0 \
--network localhost mysql:5.7
docker container run -it --name ping_mysql \
--network localhost --network bridge centos:centos7
---------------------
хранилище
docker volume -h
docker volume ls
docker volume create <NAME>
docker volume inspect <NAME>
docker volume rm
docker volume prune

Using Bind Mounts
docker container run -d --name <NAME> --mount type=bind,source=<SOURCE>,target=<TARGET> <IMAGE>
docker container run -d --name <NAME> -v <SOURCE>:<TARGET> <IMAGE>
docker container run -d --name nginx-bind-mount1 \
--mount type=bind,source="$(pwd)"/target,target=/app nginx

docker container run -d \
--name nginx-bind-mount2 \
-v "$(pwd)"/target2:/app nginx

Using Bind Volumes
docker volume create html-volume
docker container run -d \
--name nginx-volume1 \
--mount type=volume,source=html-volume,target=/usr/share/nginx/html \
 nginx

docker container run -d \
--name nginx-volume3 \
--mount type=volume,source=html-volume,target=/usr/share/nginx/html,readonly \
 nginx
-----------------
DOCKERFILE
git clone https://github.com/linuxacademy/content-weather-app.git src
docker image build -t linuxacademy/weather-app:v1 .    # run in directory with Dockerfile
docker image ls
docker container run -d --name weather-app1 -p 8081:3000 linuxacademy/weather-app:v1

ENV
--env <KEY>=<VALUE>
ENV <KEY>=<VALUE>
ENV <KEY> <VALUE>
---
FROM node
LABEL org.label-schema.version=v1.1
ENV NODE_ENV="development"
ENV PORT 3000
RUN mkdir -p /var/node
ADD src/ /var/node
WORKDIR /var/node
RUN npm install
EXPOSE $PORT
CMD ./bin/www
---------------
docker image build -t linuxacademy/weather-app:v2 .
docker container run -d --name weather-app2 -p 8082:3001 --env PORT=3001 linuxacademy/weather-app:v2
docker container run -d --name weather-app3 -p 8083:3001 --env PORT=3001 --env NODE_ENV=production linuxacademy/weather-app:v2

docker container stop $(docker container ls -aq)
docker container prune -f
---
Build ARG
--build-arg <NAME>=<VALUE>
ARG <NAME>=<DEFAULT_VALUE>

docker image build -t linuxacademy/weather-app:v3 .
docker container run -d --name weather-app1 -p 8083:3001 linuxacademy/weather-app:v3

NON-privileged User
docker image build -t centos7/nonroot:v1 .
docker container run -it --name test-build centos7/nonroot:v1 /bin/bash
ROOT
docker container exec -u 0 -it test-build /bin/bash
git clone https://github.com/linuxacademy/content-weather-app.git src
docker container run -d --name weatherapp-nonroot -p 8086:3000
docker container run -d --name weatherapp-nonroot -p 8086:3000 linuxacademy/weather-app-nonroot:v1

WORKING WITH VOLUME

