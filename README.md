# Docker-Tutorial

## Notes

Docker images - instance of an app/libraries installed inside the docker container.

Docker container - the main docker where the web app, images (e.g. apache) are installed.

## Commands

docker run "appname" -  will run the instance of an app inside the docker container.

docker ps - show available docker containers.

docker ps -a show runnning docker containers, ids, name etc..

docker stop "container_name" - will stop specific docker container.

docker rm "container_name" - remove completely the container.

### Images

docker images - show the list of installed images and their sizes.

docker rmi "app_name" - remove specific images e.g. apache etc.

                      - stop and delete running container that depends on that image before deleting completely an image.
                      
                      - 
