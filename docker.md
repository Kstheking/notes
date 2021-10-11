# Docker

a container is simply another process on your machine that has been isolated from all other processes on the host machine

[beginner's guide](https://www.freecodecamp.org/news/a-beginners-guide-to-docker-how-to-create-your-first-docker-application-cc03de9b639f/)

## Container Image 
When running a container, it uses an isolated filesystem. This custom filesystem is provided by a container image. Since the image contains the container’s filesystem, it must contain everything needed to run an application - all dependencies, configuration, scripts, binaries, etc. The image also contains other configuration for the container, such as environment variables, a default command to run, and other metadata.

## [workdir](https://stackoverflow.com/questions/55108649/what-is-app-working-directory-for-a-dockerfile)

## Dockerfile 
 A Dockerfile is simply a text-based script of instructions that is used to create a container image.
 [dockerfile reference](https://docs.docker.com/engine/reference/builder/)

## Commands 

### Images

 - list all images: `docker images`
 - delete an image: `docker image rm imageName`
 - delete all images: `docker image rm $(docker images -a -q)`

### containers

 - list all existing containers: `docker ps -a`
 - stop a container: `docker stop zen_banzai` //use the name or the id 
 - stop all running containers: `docker stop $(docker ps -a -q)`
 - remove a container: `docker rm zen_banzai` 
 - stop and remove a container in one command: `docker rm -f zen_banzai`
 - display logs of a conainer : `docker logs zen_banzai`
 - run a command in a running container : ` docker exec <container-id> cat /data.txt`

### dockerhub 

- login to dockerhub through command line : `docker login -u kstheking`
- push to dockerhub : `docker push kstheking/demo`

### Tags

- giving an image a new name: `docker tag getting-fucking-started:latest kstheking/demo`

### running an image 

 `docker run -d -p 80:80 docker/getting-started`

 - `-d`: run the container in detached mode (background) 
 -  `-p 80:80`: map port 80 of the host to port 80 in the container 
 -  `docker/getting-started` the image name

### volumes 

- create a volume : ` docker volume create todo-db`
- specify volume mount: `docker run -dp 3000:3000 -v todo-db:/etc/todos getting-started` 
  we are mounting the named volume to /etc/todos where the data to be persisted is stored 
- inspecting volume: `docker volume inspect todo-db`
The Mountpoint is the actual location on the disk where the data is stored.


## container's filesystem
When a container runs, it uses the various layers from an image for its filesystem. Each container also gets its own “scratch space” to create/update/remove files. Any changes won’t be seen in another container, even if they are using the same image.

## Container volumes 
Volumes provide the ability to connect specific filesystem paths of the container back to the host machine.
By creating a volume and attaching (often called “mounting”) it to the directory the data is stored in, we can persist the data

### named volume 
Think of a named volume as simply a bucket of data. Docker maintains the physical location on the disk and you only need to remember the name of the volume. Every time you use the volume, Docker will make sure the correct data is provided.

### Bind mounts 
With bind mounts, we control the exact mountpoint on the host. We can use this to persist data, but it’s often used to provide additional data into containers. When working on an application, we can use a bind mount to mount our source code into the container to let it see code changes, respond, and let us see the changes right away.

### start a dev-mode container 

```
docker run -dp 3000:3000 \
     -w /app -v "$(pwd):/app" \
     node:12-alpine \
     sh -c "yarn install && yarn run dev"
```

here
-  `-w /app` : sets the “working directory” or the current directory that the command will run from
-  `-v "$(pwd):/app"` - bind mount the current directory from the host in the container into the /app directory (Note: this app directory actually refers to the app directory **inside** the container
-   `node:12-alpine` the base image
-   `sh -c "yarn install && yarn run dev"` - the command. We’re starting a shell using sh (alpine doesn’t have bash) and running yarn install to install all dependencies and then running yarn run dev.

## Multi containers 
If two containers are on the same network, they can talk to each other. If they aren’t, they can’t.
There are two ways to put a container on a network: 1) Assign it at start or 2) connect an existing container. 

### create network 
` docker network create todo-app`

#### use mysql 
```
docker run -d \
     --network todo-app --network-alias mysql \
     -v todo-mysql-data:/var/lib/mysql \
     -e MYSQL_ROOT_PASSWORD=secret \
     -e MYSQL_DATABASE=todos \
     mysql:5.7
```

(here the named volume gets automatically created) 

#### connect to mysql database 

```
 docker exec -it <mysql-container-id> mysql -u root -p
 //to show databases use `SHOW DATABASES`
```

### [nikolaka netshoot](https://github.com/nicolaka/netshoot) 
a container which contains lots of network debugging and troubleshooting tools 

#### start a new container connected to some network 
`docker run -it --network todo-app nicolaka/netshoot`

#### dig 
dig command is used as a dns tool, can look up ip address of hostnames 
`dig mysql`   
here you see we use mysql which is the network alias we created in above command 

### running with env variables 

```
docker run -dp 3000:3000 \
   -w /app -v "$(pwd):/app" \
   --network todo-app \
   -e MYSQL_HOST=mysql \
   -e MYSQL_USER=root \
   -e MYSQL_PASSWORD=secret \
   -e MYSQL_DB=todos \
   node:12-alpine \
   sh -c "yarn install && yarn run dev"
```

## docker compose 

the service name can be anything and automatically becomes a network alias  
[compose file conf options](https://docs.docker.com/compose/compose-file/compose-file-v3/#compose-file-structure-and-examples)

### a sample docker-compose.yml file 

```
version: "3.7"

services: 
    app:
        image: node:12-alpine
        command: sh -c "yarn install && yarn run dev"
        ports: 
            - 3000:3000
        working_dir: /app
        volumes: 
            - ./:/app
        environment: 
            MYSQL_HOST: mysql
            MYSQL_USER: root
            MYSQL_PASSWORD: secret
            MYSQL_DB: todos

    mysql: 
        image: mysql:5.7
        volumes: 
            - todo-mysql-data:/var/lib/mysql
        environment: 
            MYSQL_ROOT_PASSWORD: secret
            MYSQL_DATABASE: todos

volumes: 
    todo-mysql-data: 
```

### starting docker compose 
` docker-compose up -d`

### to stop it all 
`docker-compose down`

### watch logs 
`docker-compose logs -f`
