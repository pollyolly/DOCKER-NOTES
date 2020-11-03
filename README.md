# Docker-Tutorial

## Notes

Docker images -instance of an app/libraries installed inside the docker container.

Docker container -the main docker where the web app, images (e.g. apache) are installed.

## Common Commands

docker run "image_name" -will run the instance of an app inside the docker container.

docker ps -show available docker containers.

docker ps -a -show runnning docker containers, ids, name etc..

docker stop "container_name" -will stop specific docker container.

docker rm "container_name" -remove completely the container.

## Images

docker images -show the list of installed images and their sizes.

docker rmi "image_name" -remove specific images e.g. apache etc. stop and delete running container that depends on that image before deleting completely an image.
    
## Run Commands

docker run "image_name" -this will download/install the image and run the container and if the image is not exists docker will automatically download it.

docker pull "image_name" -it will just download/install the image and will not run the container. Use this when we do not want to wait to download the image from docker run command.
```
Note:
    A Docker container only runs when there is an active running service inside the container. e.g apache.
    It will automatically exit when the running services stops inside the container.
```
## Append Command

docker run ubuntu sleep 5 -this command will run the container and sleep for 5 seconds and then exit.

docker exec "container_name" cat /etc/hosts -executing this command on running instance of container and will print the hosts fit.

## Run -attach - detach

docker run pollyolly/web-app -this command will run the docker image (pollyolly/web-app) in the FOREGROUND then simply Ctrl + C will quit the container and stop the app inside the container.

docker run -d pollyolly/web-app -this command will run the docker image at the background. 

docker attach "docker_id" -this command will attach you to the specific docker container.

## Docker Tag 

docker run apache:1.0 -this command will run the specific version of that image. If that is not provided it will run the default version which is the latest version.
