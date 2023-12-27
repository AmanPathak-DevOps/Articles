---
title: "Deploy MERN Stack Application using Docker & Docker Compose"
datePublished: Fri Dec 15 2023 15:51:09 GMT+0000 (Coordinated Universal Time)
cuid: clqnf2cq4000i08l64vje56sg
slug: deploy-mern-stack-application-using-docker-docker-compose-d3eec3d9d1eb
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703659809736/158c935b-c5c5-4dfd-94c3-ad02341059fc.gif

---

### Introduction

Embark on a journey into the world of containerization as we delve into the deployment of a robust MERN stack application using Docker and Docker Compose. In this blog post, we will explore two distinct approaches — Dockerfile and Docker-Compose — each bringing its own set of advantages to the deployment process.

GitHub Repository (**Dockerfile** Implementation)- [https://github.com/AmanPathak-DevOps/Docker-Projects/tree/master/Dockerfile-Project](https://github.com/AmanPathak-DevOps/Docker-Projects/tree/master/Dockerfile-Project)

GitHub Repository (**Docker-Compose** Implementation)- [https://github.com/AmanPathak-DevOps/Docker-Projects/tree/master/DockerCompose-Project](https://github.com/AmanPathak-DevOps/Docker-Projects/tree/master/DockerCompose-Project)

To deploy our application, we will use two approaches.

**Dockerfile**: According to our project, Dockerfile will help me to create images and containers but the issue you will see is that I need to provide the network for each application to communicate with each other.

**Docker-Compose**: While doing deployment using Docker-Compose, it will create the default network in which the container will be created. So, you don’t need to configure the network manually.

There are many more differences between Dockerfile & Docker-Compose. But today’s blog is dedicated to Project only. So, we won’t do that.

### Deploy using Dockerfile

Open the backend folder and check the .env file.

In the .env file, I have added the MySQL container name as the host to make a connection between the backend and the database. You can change the host name but make sure to keep the name the same as the MySQL container name and the same things for other variables.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659771996/155909c4-2abe-497e-9431-ccf8e27bc6a7.png)

Now, let’s deploy our application on containers.

First of all, we need our network in which all three containers will be running and it is necessary because all three containers need to access each other.

Create a network

docker network create three-tier-network

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659773449/d33c0bb2-7a02-4767-8100-5fec2f55e312.png)

Now, Our data is a crucial thing that needs to stay for a long. But if the container is stopped or fails then we will lose our all application data.

Create a volume

docker volume create mysql-data

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659775271/7c2ed837-6b91-4c48-8a99-5de92fdf03be.png)

As our major pre-requisites are completed. Let’s create a Dockerfile for each application which includes frontend(reactjs), backend(nodejs), and database(mysql).

Let’s start with the MySQL database.

This is our database Dockerfile.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659777134/e3b86098-4826-4328-a6ab-add4ecc400d4.png)

Explanation of database Dockerfile:

FROM mysql:latest

This command will download the MySQL image from dockerhub.

ENV MYSQL\_ROOT\_PASSWORD\=mysql123  
ENV MYSQL\_DATABASE\=school

This will set the environment variable for our database configurations.

EXPOSE 3306

This will expose the port. So that, the backend will be able to reach out to the database.

VOLUME /var/lib/mysql

To get rid of data lost, we will use volume. Hence, we have added volume to our mysql container.

CMD \[“mysqld”\]

This will start the mysql service

Now, let’s build the MySQL image & run the container

docker build -t mysql-image .

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659778768/b6459d24-6cc8-4c0b-a914-dbb091ef6406.png)

Run the mysql container

docker run — name mysql-container — network=three-tier-network -p 3306:3306 -v mysql-data:/var/lib/mysql -d mysql-image

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659780548/5924e5fb-7c88-4cf4-955d-da3f13a4bcd4.png)

Now, get into the MySQL docker container by running the below command.

docker exec -it <container\_id/container\_name> /bin/bash

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659782273/d09f5b0b-1161-443d-b8c4-9a67d8c0a27c.png)

Now, we need to create two tables to store our data in the particular table. Use the below command.

USE school;  
CREATE TABLE student (id INT AUTO\_INCREMENT PRIMARY KEY, name varchar(40), roll\_number int, class varchar(16));  
CREATE TABLE teacher (id INT AUTO\_INCREMENT PRIMARY KEY, name varchar(40), subject varchar(40), class varchar(16));

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659783621/cb097cb7-7f4c-42c2-b0f6-a2cdb82c7952.png)

The database is set and ready to connect with the backend.

Let’s deploy our backend application on the docker container.

This is our backend Dockerfile.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659785459/3b86f4e2-abd1-401f-9bc9-a5f598d81228.png)

Explanation of backend Dockerfile:

FROM node:14

This command will download the node image from dockerhub.

WORKDIR /app

It will set up our work directory which will be an /app.

COPY package\*.json ./

This command will copy both the package files to resolve the dependencies issues.

RUN npm install

This command will install the dependencies that we have in the package files.

COPY . .

This command will copy all the files that are present in the current directory to the /app directory.

EXPOSE 3500

This command will expose the port to allow the frontend container to make communication.

CMD \[“node”, “server.js”\]

This command will run our backend application.

Now, let’s build the backend image & run the backend container

docker build -t backend .

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659787161/5d128846-32b9-4930-a05e-c7d3781c18b8.png)

docker run -d -p 3500:3500 — name backend-container — network=three-tier-network backend

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659788724/ed03d025-b2b5-4a02-9b9f-0b9f779c1b2d.png)

To validate, whether your backend application is running fine or not. Hit the localhost:3500 on your favorite browser.

As you can see, the backend application is responding as we expected.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659790312/3bf983c6-367d-4af3-aea6-f4eb18507741.png)

Let’s deploy our frontend application on the docker container.

This is our frontend Dockerfile.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659791519/6f6f0dff-06db-405d-b290-51c8fbcc4421.png)

In the frontend Dockerfile, we are implementing multi staging, because our application is reactjs based and we need to host it on our Nginx web server.

Explanation of frontend Dockerfile:

FROM node:21\-alpine3.17 as build

This command will download the node image on the local machine and the entire execution will be built to use it in the next stage.

WORKDIR /app

It will set up our work directory which will be an /app.

COPY . .

This command will copy all the files that are present in the current directory to the /app directory.

RUN npm install

This command will install the dependencies that we have in the package files.

RUN npm run build

This command will create the build directory in which our content is present to host on the nginx server.

Here, our first stage is completed. Now, we have to shift our content to the */usr/share/nginx/html* folder.

FROM nginx:1.25.3\-alpine

This command will download the nginx image.

COPY — from=build /app/build /usr/share/nginx/html

This command will copy all the files that are present inside the build directory to /usr/share/nginx/html*.*

EXPOSE 80

This command will expose the port. So, we will be able to access the front end.

CMD \[“nginx”, “-g”, “daemon off;”\]

This command will run the nginx web server in the foreground and make sure to keep running in the foreground.

Now, let’s build the frontend image & run the container

docker build -t frontend .

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659793555/45004ec7-3386-4eb8-bbe5-1f417658d529.png)

docker run -d — name frontend-container — network=three-tier-network -p 80:80 frontend

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659795244/fa6f8f6c-d55c-47b7-bb21-d67886130e1d.png)

If you hit the localhost:80 or just localhost on your favorite browser. You will see the magic.

Click on Student.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659797026/791d6a1b-991c-4da9-86ed-2ee258c4b90d.png)

Enter the data and click on Submit.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659798821/45e28f51-c2aa-4370-8ca2-e06be9418c82.png)

You will see the data is inserted and you will be able to view it. If you want to delete the data, kindly click on the delete button.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659800938/3a0db02e-28b8-4e3a-b487-0c85869e111b.png)

We have provided the volume to the database container. So, if the container is deleted, then don’t worry the data will be still available and it will be automatically added to the new docker container, you just need to provide the same docker volume.

### Deploy using Docker-Compose file

This is it for multiple Dockerfiles.

However, the process is a little bit lengthy by creating each image and creating each container.

To get rid of this issue, I will use docker-compose here which will help me to create the docker image and create the container of that image in one go.

Let’s do that.

Before going to do the demo for docker-compose, make sure that all three docker images and containers are deleted.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659802654/8c9752ec-8f0b-4549-aeff-5de8e9ba8616.png)

In docker-compose, you don’t need to create a network or volume manually. docker-compose will take care of those things.

The Dockerfile for frontend and backend will remain the same. You can see the file structure and docker-compose.yml file in below screenshot and keep it as it is.

**Note:** Ignore the error showing in the docker-compose file because docker is not installed in the VS code.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659804430/5f1f230e-af8d-4aaa-91ce-7de12a5df9e3.png)

docker-compose file explanation:

This specifies the docker-compose file format.

version: ‘3’

In the below code, we are creating a frontend container where the Dockerfile will be taken from the frontend directory. The frontend container will be running on port 80 and the container will depend on the backend container which means the backend container will be created first then, only the frontend container will be created.

services:  
frontend:  
container\_name: frontend  
build:  
context: frontend/  
image: fronted-image  
ports:  
\- "80:80"  
depends\_on:  
\- backend

In the below code, we are creating a backend container where the Dockerfile will be taken from the backend directory. The backend container will be running on port 3500 and the container will depend on the database container which means the database container will be created first then, only the backend container will be created. Also, there are some environment variables are added to use for the backend code.

backend:  
container\_name: backend  
build:  
context: backend/  
image: backend-image  
ports:  
\- "3500:3500"  
environment:  
\- MYSQL\_HOST=mysql-container  
\- MYSQL\_USER=root  
\- MYSQL\_PASSWORD=mysql123  
\- MYSQL\_DATABASE=school  
depends\_on:  
\- db

In the below code, we are creating a database container where the Dockerfile is not necessary. The database container will be running on port 3306. Also, there are some environment variables are added to use for the MySQL database configurations and we have added the volume to the mysql container to get rid of data loss.

db:  
container\_name: mysql-container  
image: mysql:latest  
environment:  
\- MYSQL\_ROOT\_PASSWORD=mysql123  
\- MYSQL\_DATABASE=school  
volumes:  
\- mysql-data:/var/lib/mysql  
volumes:  
mysql-data:

Let’s try to deploy our application using docker-compose

There is one pre-requisite for docker-compose docker-compose must be installed on your machine. If not, then use the below command to install it.

apt install docker-compose -y

After installing the docker-compose, run the docker-compose file to deploy your application.

docker-compose up -d

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659805823/304a505b-0f80-4e31-aeca-a5b1568f6865.png)

Now, hit the localhost on your favorite browser and enjoy the application.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703659808066/037b8e48-2c89-4493-8e52-1ec2cb03f715.png)

### Conclusion

In the dynamic landscape of container orchestration, deploying a three-tier MERN stack application unveils a captivating journey. Whether opting for Dockerfile’s granular control or Docker-Compose’s cohesive approach, this blog equips you with the knowledge to navigate the intricate deployment process. Embrace containerization, fortify your applications, and set sail into the seamless world of Docker deployment.

**Feel free to reach out to me if you have any doubts or queries.**

Profile-**LinkedIn**\- [https://www.linkedin.com/in/aman-devops/](https://www.linkedin.com/in/aman-devops/)

**GitHub**\- [https://github.com/AmanPathak-DevOps/](https://github.com/AmanPathak-DevOps/)

**Profile-** [https://linktr.ee/Aman\_Pathak](https://linktr.ee/Aman_Pathak)

Happy Learning!

Aman Pathak

### Stackademic

*Thank you for reading until the end. Before you go:*

*   *Please consider* ***clapping*** *and* ***following*** *the writer! 👏*
*   *Follow us on* [***Twitter(X)***](https://twitter.com/stackademichq)*,* [***LinkedIn***](https://www.linkedin.com/company/stackademic)*, and* [***YouTube***](https://www.youtube.com/c/stackademic)***.***
*   *Visit* [***Stackademic.com***](http://stackademic.com/) *to find out more about how we are democratizing free programming education around the world.*