### Docker Compose

$docker-compose up -d
```
#docker-compose.yml
version: '3.3'                                                                                                                                                                                                                             
services:
     mydatabase:
          image: mysql:5.7
          volumes:
               - mydatabase_data:/var/lib/mysql
          restart: unless-stopped
          container_name: docker_wordpress_mysql
          env_file: .env
          environment:
               - 'MYSQL_DATABASE: $MYSQL_DB_NAME'
#               - MYSQL_USER: $MYSQL_USER
#               - MYSQL_PASSWORD: $MYSQL_PASSWORD
          networks:
               - app-network

     wordpress:
          depends_on:
               - mydatabase
          image: wordpress:latest
          ports:
               - "8001:80"
          restart: unless-stopped
          env_file: .env
          container_name: docker_wordpress
          environment:
               - 'WORDPRESS_DB_HOST: mydatabase:3306'
               - 'WORDPRESS_DB_USER: $MYSQL_USER'
               - 'WORDPRESS_DB_PASSWORD: $MYSQL_PASSWORD'
               - 'WORDPRESS_DB_NAME: $MYSQL_DB_NAME'
          volumes:
               - wordpress_data:/var/www/html/docker_wordpress
          networks:
               - app-network
volumes:
      mydatabase_data:
      wordpress_data:
networks:
     app-network:
          driver: bridge
```
```
#.env
MYSQL_USER=root
MYSQL_PASSWORD=!!++labsMYSQL--##
MYSQL_DB_NAME=docker_wordpress
```
