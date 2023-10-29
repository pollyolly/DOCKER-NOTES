# Docker-Tutorial
```
https://www.freecodecamp.org/news/the-docker-handbook/
```
## Notes

Docker images -instance of an app/libraries installed inside the docker container.

Docker container -the main docker where the web app, images (e.g. apache) are installed.

## Common Commands
```vim
docker run "image_name" -will run the instance of an app inside the docker container.

docker ps -show runnning docker containers, ids, name etc..

docker ps -a -show available docker containers.

docker stop "container_name" -will stop specific docker container.

docker rm "container_name" -remove completely the container.

docker exec -it cc3ed4c1304a /bin/bash   -access container inside docker.
```
## Images
```vim
docker images -show the list of installed images and their sizes.

docker rmi "image_name" -remove specific images e.g. apache etc. stop and delete running container
                         that depends on that image before deleting completely an image.
```    
## Run Commands
```vim
docker run "image_name" -this will download/install the image and run the container and if the image is not exists docker will automatically download it.

docker pull "image_name" -it will just download/install the image and will not run the container.
                          Use this when we do not want to wait to download the image from docker run command.
```
```
Note:
    A Docker container only runs when there is an active running service inside the container. e.g apache.
    It will automatically exit when the running services stops inside the container.
```
## Append Command
```vim
docker run ubuntu sleep 5 -this command will run the container and sleep for 5 seconds and then exit.

docker exec "container_name" cat /etc/hosts -executing this command on running instance of container and will print the hosts fit.
```
## Run -attach - detach
```vim
docker run pollyolly/web-app -this command will run the docker image (pollyolly/web-app) in the
                              FOREGROUND then simply Ctrl + C will quit the container and stop the app inside the container.

docker run -d pollyolly/web-app -this command will run the docker image at the background. 

docker attach "docker_id" -this command will attach you to the specific docker container.
```
## Docker Tag 
```
docker run apache:1.0 -this command will run the specific version of that image.
                       If that is not provided it will run the default version which is the latest version.
```
## RUN - STDIN (Standard Input)

```vim
Note:
     Docker doesn't read standard input mode or when a script (.sh) ask for input.
     Docker will ignore the input and print the output even the script (.sh) asked for input.
     Docker doesn't allow interactive mode by default.
```
```vim
docker run -i pollyolly/my-script-app   -this command (-i parameter) will allow the interactive mode.

docker run -it pollyolly/my-script-app  -this command (-it parameter) will allow the interactive and terminal mode to ask the input in the terminal.
```
## PORT MAPPING
```vim
Docker container -where our app installed. Our container runs in 192.168.5.30 in port 5000.
Docker host -where our containers are inside. Our host runs in 192.168.1.20 in port 80.

docker run -p 80:5000 pollyolly/web-app  -this command will map the port 80 to port 5000 (we can now read our app in port 80 while our docker container runs in port 5000). 
                                          We can now use the IP of docker host to access our app outside. 192.168.1.20:80
```
```
Note:
     You cannot run on the same port with docker host. e.g. docker run -p 3360:3360.
     You can have multiple have running in different ports.
```
## VOLUME MAPPING
```vim
docker run -v /opt/datadir:var/lib/mysql mysql  -this command will copy/mount the speicified container to another location. 
                                                 This will help to not delete completely the container or images when we remove or stop the container.
```
## Inspect Container
```vim
docker inspect "container_name"  -this command will show the information of that container.
```
## Logs

docker logs "container_name"   -this command will show the logs of container specially when running background.

## Environment Variables
-use this to add configurable variable inside your app.
```
app.py

import os

color = os.environment.get('APP_COLOR')
print color
```
export APP_COLOR=blue; python app.py  -this command will set the environment variable (env) of the script in the docker.

docker run "container_name"  -check the web-app if env take effect.

docker run -e APP_COLOR=blue "container_name"  -this command will set the environment variable (env) of the script in the docker while running.

docker inspect "container_name" -display the current Environment variables of docker.
```
{
    ...
   "Config":{
        "Env": {
             "APP_COLOR=blue"
         }
   }
   ...
}

```
run multiple instance to have different color in three web-app.
```
docker run -e APP_COLOR=blue "container_name" 
docker run -e APP_COLOR=green "container_name" 
docker run -e APP_COLOR=red "container_name" 
```
## Creating Images
```
Planning steps for the instruction in Docker.
1. OS - ubuntu
2. Update apt repo
3. Install dependencies using apt
4. Install python dependencies in pip
5. Copy source code to /opt folder
6. Run web server using 'flask' command
```
Setup Docker file

Filename: Dockerfile
```
FROM Ubuntu

RUN apt-get update
RUN apt-get install python

RUN pip install flask
RUN pip install flask-mysql

COPY . /opt/source-code

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
```
docker build Dockerfile -t pollyolly/web-app  -this command will build the docker image using the "Dockerfile" config/commands.

docker push pollyolly/web-app  -this command will push your docker to docker hub.docker.com

docker images -shows the list of created images

## Creating Containers
```
docker images
docker run -d dockerized_app:latest  -this will create the container for the image dockerized_app:latest
docker run -tid --name="name_of_container" dockerized_app:latest  -this will re-create and rename the container
```
List available images and create the container then replace dockerized_app:latest(repository:tag) selected on the images list.

## Layered Architecture
```
-----
FROM Ubuntu                   -start from the base os
-----
RUN apt-get update            -install dependencies
RUN apt-get install python

RUN pip install flask
RUN pip install flask-mysql
-----
COPY . /opt/source-code       -copy the source code in opt/source-code folder inside the docker image.
-----
ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run         -this will specify a command that will be run when the image run as a container.
```
## Failure 
```
Layer 1. OS - ubuntu
Layer 2. Update apt repo
Layer 3. Install dependencies using apt                    -IF THIS LAYER FAILED <read below>
Layer 4. Install python dependencies in pip
Layer 5. Copy source code to /opt folder
Layer 6. Run web server using 'flask' command

If failure occured by re-running using the command "docker build Dockerfile -t pollyolly/web-app" will re-use the previous cached steps and continue to the remaining layers.
````
    
