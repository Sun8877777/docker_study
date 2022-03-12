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
