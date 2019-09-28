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
docker cotnainer stop $containerid  ; stop container
docker container ls -a ; list all containers
```

Commands
```
docker container logs webhost  ; get lastest logs
docker container top webhost   ;
```

Remove container
```
docker container rm -f $containerid
```

### Wat's going on in container
```
docker container top  ; process list
docker container inspect  ; details of container config
docker container stats  ; live performance stats
```

### Resources

[Docker mastery](https://www.udemy.com/course/docker-mastery/)
