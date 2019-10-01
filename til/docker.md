### Theory

`Image` - the application we want to run
`Container` - an instance of that image running as a process

### Install

Edge release
```
curl -sSL https://get.docker.com/ | sh
```

[Stable distro](store.docker.com)

Проверить работоспособность
```
sudo docker version
```

Добавить себя в группу. Нужно разлогиниться и снова зайти.
```
sudo usermod -aG docker $USER
```

Ставим Docker Machine [link](https://docs.docker.com/machine/install-machine/)

Ставим Docker Compose [link](https://docs.docker.com/compose/install/)

### Information

```
docker version  ; verify cli
docker info     ; get configj
docker          ; list of commands
```

### Example (nginx)
Workflow
```
docker container run --publish 80:80 --detach --name webhost nginx  ; run nginx
docker container ls  ; list running containers
docker cotnainer stop <container>  ; stop container
docker container ls -a ; list all containers
```

Commands
```
docker container logs webhost  ; get lastest logs
docker container top webhost   ;
```

Remove container
```
docker container rm -f <container>
```

### Wat's going on in container
```
docker container top  ; process list
docker container inspect  ; details of container config
docker container stats  ; live performance stats
```

### Getting inside container
```
docker container run -it  ; start new container interactively
docker container exec -it  ; run command in existing container
```

### Docker Networks
```
docker container port <container>  ; check open ports
```

### Managing networks
```
docker network ls               ; show networks
docker network inspect          ; details about network
docker network create --driver  ; create network
docker network connect          ; attach network to container
docker network disconnect       ; detach network from container
```

Default network: `bridge`

Example create network and attach container
```
docker network create my_app_net
docker run -d --name new_nginx --network my_app_net nginx
```

### DNS

Если мы используем сеть отличную от `bridge`, можем не заботиться об DNS и обращаться к контейнерам по имени.
```
docker container exec -it my_container_1 ping my_container_2
```

В `bridge` сети можно использовать `--link`. Но проще просто создать  новую сеть.

### Images

```
docker image ls          ; list images
docker pull <image:tag>  ; download image

docker history <image:tag>  ; get history of changes in the image
```

```
docker image build -t <name:tag> .
```

```
docker image prune -a  ; remove all unused images
docker system prune    ; clean everything
```

### Resources

[Docker mastery](https://www.udemy.com/course/docker-mastery/)
