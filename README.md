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



    
        
      
