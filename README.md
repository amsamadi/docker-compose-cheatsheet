# docker-compose-cheatsheet
### This document contains some common questions and their answers about docker-compose
---
### 1- What is docker-compose
* Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration
---
### 2- Write a docker compose to build X project in current directory and deploy it
* In this example we build a nginx image with certbot and deploy it under the name of webserver

##### Docker Compose
Syntax:

     version: !!str 3
     services:
       nginx-certbot:
         build:
           context: .
         image: nginx-certbot:v.1.0
         container_name: webserver
         ports:
           - "80:80"
           - "443:443"
##### Dockerfile
Syntax:

    FROM nginx:1.20.1
    RUN apt update && apt install python3 python3-venv libaugeas0 -y && rm -rf /var/lib/apt/lists/*
    RUN python3 -m venv /opt/certbot/ &&\
       /opt/certbot/bin/pip install --upgrade pip &&\
       /opt/certbot/bin/pip install certbot certbot-nginx &&\
       ln -s /opt/certbot/bin/certbot /usr/bin/certbot
---
### 3- Write a docker compose to run Mysql and PhpMyadmin in production way
Syntax:

    version: !!str 3
    services:
      mysql:
        image:  mysql:8.0
        container_name: mysql
        hostname: mysql
        restart: unless-stopped
        environment:
          - MYSQL_ROOT_PASSWORD=changeme
          - MYSQL_DATABASE=test
        volumes:
          - mysql_data:/var/lib/mysql/
        stop_grace_period: 30s
        healthcheck:
          test: "mysqladmin ping -h localhost -uroot -p$MYSQL_ROOT_PASSWORD"
          interval: 5s
          timeout: 20s
          retries: 5
        networks:
          - database
      phpmyadmin:
        image: phpmyadmin
        restart: unless-stopped
        container_name: phpmyadmin
        hostname: phpmyadmin
        depends_on:
          - mysql
        environment:
          - PMA_HOST=mysql
          - PMA_PORT=3306
          - MYSQL_ROOT_PASSWORD=changeme
        restart: unless-stopped
        ports:
          - "8080:80"
        networks:
          - database
    networks:
      database:
    volumes:
      mysql_data:
      
---
### 4- How to join containers to pre-existing networks?

Syntax:

    services:
      # ...
    networks:
      default:
        external: true
        name: my-network

* Instead of attempting to create a network called [projectname]_default, Compose looks for a network called my-network and connect your app’s containers to it
---
### 5- How to scale an aplication?
* To do this, we must use a load balancer to divide the load between different instances of the application, and also in the compose-file , a port should not be published and the container_name should not be set. 

* for this we use a following command:

Syntax:

     docker-compose scale SERVICE=NUMBER_OF_INSTANCES
---
### 6- How to view the logs of stack that deployed with Docker Compose?
* for this we use a following command:

Syntax:

    docker-compose logs [SERVICE]

    
        
      
