### Docker Images
```
#build image
$cd http-server/
$docker build -t image-name .
#images list
$docker images
#remove image
$docker rmi image-id
#tag image #docker repository:tag repository:new_tagname
$docker http-server:latest http-server:v1
#remove referenced repository
$docker rmi http-server:v1
#push image to dockerhub
$docker push http-server:latest
```
### Docker Container
```
#deploy image to container 
#-d DETACH run container in background 
#-p publish PORT mapping
#Host 0.0.0.0:8080 access to URL #Container and Application port 3000 
#--name container-name
$docker run -dp 0.0.0.0:8080:3000 image-name
# docker stop / start / pause / unpause container-id
$docker stop container-id
#list all container
$docker ps -a
#enter container to edit files
#inside the container install apt-get update then apt-get install vim to edit
$docker exec -it container-id bash
#remove docker container
$docker rm container-id
```
### Docker Volume
```
#list docker volume
$docker volume ls
#create volume
$docker volume create volume-name
#remove volume
$docker volume rm volume-name
#check details of volume
$docker volume inspect volume-name
#create container from image and mounted volume
#docker run -dp host_port:container_port -v volume_name:/path/ image-name
#--name container-name
$docker run -dp 9000:4000 -v http-server-files:/http-server/uploads http-server
#test copy file from host to container volume
$docker cp test.txt container_name:/http-server/uploads
#test copy from container volume to host external folder
$docker cp crazy_mahavira:/http-server/uploads/test.txt /path/external
#mounting from host folder (./uploads) to inside container /external/uploads folder
$docker run --name http-server-container -dp 9000:4000 -v ./uploads:/external/uploads http-server
```
### Dockerfile
```
FROM -> base image image_name:tag 
     -> FROM node:latest
WORKDIR -> Work directory for commands (RUN, CMD, ENTRYPOINT, COPY, ADD)
        -> cd inside to the path inside container i.e. cd /webapp
        -> WORKDIR /webapp; This will run the commands inside the container path /webapp
RUN  -> command 1 && command 2 i.e. RUN apt-get update && apt-get install msmtp
ADD  -> copy from source/local - to destination container; support archived files tar, zip etc.
     -> ADD package.json . #if WORKDIR specified a path
     -> ADD package.json /webapp #if WORKDIR does not exists
COPY -> copy from source/local - to destination container;
     -> COPY testfile.js . #if WORKDIR specified a path
     -> COPY testfile.js /app #if WORKDIR does not exists
VOLUME -> VOLUME ../webapp/uploads #inside webapp folder
CMD  -> execute, param, param i.e. CMD["node","run","dev"] or CMD node run dev
ENTRYPOINT -> execute, param, param i.e. ENTRYPOINT["node","./index.js"]
ENV -> APP_URL=http://localhost:8080
EXPOSE 8080 -> expose port 8080 to public
```
### Creating Network (Bridge)
```
#list network
$docker network ls
#check network config
$docker network inspect network-name
#network create
$docker network create http-server-network
#connect container-name network connect, disconnect
$docker network connect network-name container-name
#test connection
$docker exec -it container-id bash
$apt-get install iputils-ping
$ping container-name
```
### Docker Compose
```
# docker-compose.yml
services:
  web:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: http-server-web
#   command: npm start
    ports:
      - 80:4000
    environment:
      DATABASE_URL: mongodb://db:27017
    volumes:
      - http-server-data:/http-server/uploads
    networks:
      - http-server-network
#    depends_on:
#      - db
    develop:
      watch:
#Action  sync -> changes from host to container matched
#     rebuild -> automatically rebuild image and replace in running container
#sync+restart -> ideal when changing config files and dont need updating image but restart main process of container
#             -> ideal when chaing nginx.conf and database configs
#      target -> target path in container
#        path -> path in host or local
        - path: ./package.json
          action: rebuild
        - path: ./package-lock.json
          action: rebuild
        - path: ./
          target: /http-server
          action: sync
          ignore:
            - node_modules/
  db:
    image: mongo:latest
    container_name: http-server-db
#   restart: always #restart
#   pull_policy: always #get updated images
    ports:
     - 27017:27017
#   environment:
#     DATABASE_URL: 
    volumes:
      - http-server-mongo:/data/db
    networks:
      - http-server-network
 
  nginx:
# /usr/share/nginx/html/ 
    image: nginx:latest
    container_name: http-server-nginx
#   restart: always
    ports:
      - 80:9000
    volumes:
      - http-server-nginx:/etc/nginx
    networks:
      - http-server-network
#   develop:
#     watch:
#       - action: sync+restart
#         path: ./nginx.conf
#         target: /etc/nginx/

volumes:
  http-server-nginx:
  http-server-data:
  http-server-mongo:
networks:
  http-server-network:
```
### Docker Compose Command
```
#run docker compose in background
docker compose up -d
#build image first
docker compose build service-name
```
### Helpful Resource

[Assign individual IP from a subnet network in Docker](https://stackoverflow.com/questions/39493490/provide-static-ip-to-docker-containers-via-docker-compose)
