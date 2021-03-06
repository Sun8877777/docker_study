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
docker image build -t linuxacademy/nginx:v1 .

---------------------
MULTY stage
docker image build -t linuxacademy/weather-app:multy-stage-build --rm --build-arg VERSION=1.5 .
------------------------
Tagging
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
docker image tag sun8877777/weather-app:b301a90001b1524de26f9fdd92694e61cf2d3724 sun8877777/weather-app:latest
docke push sun8877777/weather-app:latest
-------------------
History
docker image history
docker image history --no-trunc
docker image history --quiet --no-trunc
--------------------
Save to .tar
docker image save <image> > <FILE>.tar
docker image save <image> -o <FILE>.tar
docker image save <image> --output <FILE>.tar

Load from .tar
docker image load < <FILE>.tar
docker image load -i <FILE>.tar
docker image load --input <FILE>.tar
-------------------
Инспектирование
docker container top <NAME>
docker container stats <NAME>
----------------------
Автоматический запуск контейнеров
--restart
    - no                        -- не перзапускать, по умолчанию
    - on-failrule               -- если есть код завершения, не равный 0
    - always                    -- всегда перезапускать
    - unless-stopped            -- перезапуск всегда, кроме того, когда вы сами остановили контейнер
docker update --restart unless-stopped <NAME> -- Изменение политики рестарта, для запущенного контейнера

docker run -d --name always-restart --restart always sun8877777/weather-app:latest
docker run -d --name nless-restart --restart unless-stopped  sun8877777/weather-app:latest
sudo systemctl restart docker
------------------------------
Docker EVENTS
docker system events
docker system events --since '<TIME_PERIOD>'
docker system events --filter <FILTER_NAME>=<FILTER>
docker system events --since '1h'
docker system events --filter type=container --since '1h'
docker system events --filter type=container --filter event=start --since '1h'
docker system events --filter type=container --filter event=attach --filter event=die  --filter event=stop --since '1h'
-----------------------------
Управление остановленными контейнерами
docker container ls -a -q
docker container ls -a -f status=exited
------------------------------
Портайнер
docker volume create portainer_data
docker container run -d --name portainer -p 8080:9000 \
--restart=always \
-v /var/run/docker.sock:/var/run/docker.sock \
-v portainer_data:/data portainer/portainer
docker container ls
----------------------------
WatchTower - перезапуск образов при изменении в них

git clone https://github.com/linuxacademy/content-express-demo-app.git watchtower
git checkout dockerfile
docker image build -t sun8877777/my-express .
docker container run -d --name watch-app -p 80:3000 --restart always sun8877777/my-express
docker container run -d --name watchtower \
--restart always \
-v /var/run/docker.sock:/var/run/docker.sock \
v2tec/watchtower -i 15
----------------------------
docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version
------------------
Compose commands
docker-compose
  - up create and start
  - ps
  - stop
  - start
  - restart
  - down -- stop and remove
----------------------
docker-compose build
--------------------
docker swarm
docker swarm init --advertise-addr [PRIVATE_IP]  -- init manager, ип объявленный здесь показывает остальным членам роя, через кого идти в апи
docker swarm join --token [TOKEN] [PRIVATE_IP]:2377
docker node ls
------------
docker swarm init --advertise-addr 192.168.88.166
docker swarm join --token SWMTKN-1-5suuu2rfwtk0z13rwzm04vx5kctr84xk9chmpv4k1i2rvbwmpu-8eh6jim6ad4z0dtqszi5yci26 192.168.88.166:2377  -- для других членов роя
docker node ls
docker node promote [id]  - повышение уровня ноды до управляющего
docker node demote [id]  - понижение уровня ноды до обычного
docker node rm -f [NODE_NAME]  delete node from swarm
docker swarm leave  - command in node(exit from swarm)
docker swarm join-token [type]((worker|manager))
-------------------
services
docker service create -d --name [name] \
-p [HOST_PORT]:[CONTAINER_PORT]\
--replicas [REPLICAS] [IMAGE] [CMD]
docker service ls
docker service inspect [NAME]
docker service logs [NAME]
----
docker service COMMAND
Commands:
  create      Create a new service
  inspect     Display detailed information on one or more services
  logs        Fetch the logs of a service or task
  ls          List services
  ps          List the tasks of one or more services
  rm          Remove one or more services
  rollback    Revert changes to a service's configuration
  scale       Scale one or multiple replicated services
  update      Update a service
-----
docker service create -d --name nginx_service -p 8080:80 --replicas 2 nginx:latest
docker service inspect nginx_service
docker service logs nginx_service
docker service scale nginx_service=3
docker service update --help
--------
Network in SWARM
docker network create  -d overlay [NAME]
docker service create -d --name \
-- network [NETWORK]
-p [HOST_PORT]:[CONTAINER_PORT] \
--replicas  [REPLICAS] \
[IMAGE] [CMD]
-----
docker service update --network-add [NETWORK] [SERVICES]
docker network create -d overlay my_overlay
docker network create -d overlay --opt encrypted encrypted_overlay - шифрованная сеть
docker service create -d --name nginx_overlay --network my_overlay -p 8081:80 --replicas 2 nginx:latest
docker service update --network-add my_overlay nginx_service
docker service update --network-rm my_overlay nginx_service
docker network rm encrypted_overlay
------
Volumes
docker plugin install [PLUGIN] [OPTION]
docker plugin ls
volumePlugins^
    Hedvig
    Pure Storage
    HPE Nimble Storage
    Nutanix DVP
    Blockbridge
    NexentaStor
    StorageOS
    Rex-Ray
---------
docker plugin install store/splunk/docker-logging-plugin:2.0.0 --alias splunk-logging-plugin
docker plugin disable [id]
docker plugin rm [id]
docker volume create -d local portainer_data
docker service create --name portainer --publish 8000:9000 \
--constraint 'node.role == manager' --mount type=volume,src=portainer_data,dst=/data \
--mount type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock \
portainer/portainer
--------------------------
Стэки
docker stack deploy --compose-file docker-compose.yaml prometheus
docker stack ls
sudo chown nobody:65534 -R prometheus.yaml 
sudo chown nobody:nogroup -R /var/lib/docker/volumes/prometheus_data
docker stack deploy --compose-file docker-compose.yaml prometheus
--------------
SECCOMP PROFILE
wget https://raw.githubusercontent.com/moby/moby/master/profiles/seccomp/default.json
docker container run --rm -it --security-opt seccomp=./default.json alpine

docker container --cap-drop=[CAPABILITY] [IMAGE] [CMD]
docker container --cap-add=[CAPABILITY] [IMAGE] [CMD]
docker container run --rm -it  alpine sh
mknod /dev/random2 c 1 8
docker container run --rm -it --cap-drop=MKNOD alpine sh
mknod /dev/random2 c 1 8

docker container run -it --cpus=[VALUE] --memory=[VALUE][SIZE] --memory-swap [VALUE][SWAP] [IMAGE] [CMD]
----------------------------------
Secrets
STDIN | docker secret create [NAME]
docker secret create [NAME] [FILE]
docker secret ls
docker secret inspect
docker service create --name [NAME] --sectet [SECRET] [IMAGE]
openssl rand -base64 20 | docker secret create my_secret -
