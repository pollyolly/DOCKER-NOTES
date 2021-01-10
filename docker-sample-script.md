```
# Pull base image
FROM ubuntu:14.04

# Install

RUN \
   sed -i 's/# \(.*mltiverse$\)/\1g' /etc/apt/sources.list && \
   apt-get update && \
   apt-get -y upgrade && \
   apt-get install -y build-essential && \
   apt-get install -y software-properties-common && \
   apt-get install -y byobu curl git htop man unzip vim wget && \
   rm -rf /var/lib/apt/lists/*

# Add files.

ADD root/.bashrc/ root/.bashrc
ADD root/.gitconfig /root/.gitconfig
ADD root/.scripts /root/.scripts

# Set environment variables.

ENV HOME /root

# Define working directory.

WORKDIR /root

# Define default command.

CMD ["bash"]
```
Inside dockerized_app in commandline type: docker build -t dockerized_app . (this will create the image for dockerized_app) 

File: dockerized_app/Dockerfile
```
#Dockerfile
FROM php:7.4-apache
COPY . var/www/html/dockerized_app
WORKDIR var/www/html/dockerized_app
EXPOSE 8002
CMD ["php", "index.php"]
```
