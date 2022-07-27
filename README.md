
# Docker

> A tool for managing containers


## Installing Docker in Windows

  - https://docs.docker.com/desktop/install/windows-install/

  - https://docs.microsoft.com/en-us/windows/wsl/install-manual


## Containers:
 are like a package that contains all app dependencies and are used to run applications in an isolated environments in a system. They are the runable instances of images.

Containers are *isolated process*. They run independently from any other process in the computer.

**Containers vs VMs:** 

- *VMs* have their own full operating system and are typically slower

- *Containers* share the host's full operating system and are typically quicker

## Docker Images

 > Like blueprints for containers and they contains every single thing that the application might need to work. Docker images are read only

Docker images stores
- Runtime environment
- Application code
- Any dependencies
- Extra configuration (e.g. env variables)
- Command

When we RUN the image, it creates a container, which is a process that can run the application exactly as outlined in the image. 

Images are made up from several *layers*
- **Parent Image:** Includes OS & sometimes the runtime environment
- **Source code**
- **Dependencies**
- **Run commands**

We can get docker images from https://hub.docker.com/

e.g. `docker pull node`

## Docker File
consists of set of instructions to Docker to create the image. It lists out all the layers or instructions to create those layers on image

e.g.

`FROM node:17-alpine`

`WORKDIR /app` 

`COPY . . `

`RUN npm install`

`EXPOSE 4000`

`CMD ["node", "app.js"] `

## Creating Docker Image

`docker build -t myapp`

The above command will create docker image from the dockerfile with all the layers

### .dockerignore
will ignore the folder or files to be added in image

## Docker Commands

- `docker images`: to list down the images
- `docker run myapp`: runs the *myapp* named image, and the container name will be randomly generated
- `docker run --name myapp_c myapp`: runs the *myapp* named image, and the container name will be *myapp-c*
- `docker run --name myapp_c -p 4000:4000 -d myapp`: runs the *myapp* named image, and the container name will be *myapp-c*. The ***-p*** flag publishes or maps the container's port to the host computer. Port on right is exposed by the container and the port of left is to be mapped on host computer. ***-d*** flag detaches the terminal from process
- `docker ps`: shows the list of running containers  
- `docker ps -a`: shows the list of all containers  
- `docker stop myapp_c`: stops the running container
- `docker start myapp_c`: starts the existing container
- `docker image rm myapp`: removes *myapp* named image. *Only the unused images can be deleted*.
- `docker image rm myapp -f`: force remove the image even if it is being used
- `docker container rm myapp`: removes the container
- `docker system prune -a`: removes all containers, images and volumes

## Layer Caching

Docker cache the image layers. E.g. If source code is changed, there is a need to create new image to apply new changes for container. The second build might take less time because of cached image layers.
If one layer changes the other layers after that need to be build again and their cached won't be used


## Volumes
Feature of docker that allow to specify folders on host computer that can be made available to running containers, and those folders can be mapped to host computer to specific folder inside container, so if something changes on our computer, that changes would also be reflected in the folders we mapped to in container. In short, volumes can be used for directory mapping between the project folders and the container so that we can see changes without rebuilding anything.

`docker run --name myapp_c -p 4000:4000 -rm -v C:\Users\Name\api:/app -v /app/node_modules myapp:nodemon`

- **-rm** flag will remove the container when its stopped.
- **-v** flag will create a volumn which maps project *api* folder with container folder *app*
- second **-v** flag is anonymous volumn would map the container's node_modules folder to somewhere else in the host computer managed by docker
- **:nodemon** is the tag of image

## Docker Compose
helps in running multiple containers that are part of same project. Its a single docker compose file `docker-compose.yaml` which contains all the container configuration of project. 

- `docker compose up`
- `docker compose down`

----------

*notes are taken from netninja* [docker playlist](https://youtube.com/playlist?list=PL4cUxeGkcC9hxjeEtdHFNYMtCpjNBm3h7)