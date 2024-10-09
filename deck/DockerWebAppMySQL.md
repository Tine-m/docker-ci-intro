# Exercise 4: Run Web App in a Docker Container

If we have a Spring Boot appplication and a MySQL database, we will need to consider app and database as two different services. 
Basically, we are going to run the services in two separate docker containers. 
We will run the two  containers in the same Docker network so that they can communicate with each other. 
In this exercise you will create a docker image for a Spring boot application, 
then create a docker container from the image and finally run the app in a docker container.

### Preconditions

This assumes you have installed Docker and Maven locally on your developer computer.

### Learning outcome

* Be able to build a Docker image   
* Be able to run image in docker container  
* Be able to run image in docker container in detached mode (background)

### MySQL Image in a Docker Container

1\) You can clone this repo as your starting point [springboot-docker-demo](https://github.com/Tine-m/spring-docker-demo)
 or use a similar repo of your own.
 
 The Spring Boot project architecture: 

```java
int i = 10;
```

2\) Let's first deploy the MySQL image in a docker container (or reuse an existing image).
Step1: Pull MySQL Image
Here is the docker command to pull the latest MySQL docker image:

```docker
docker pull mysql
```


#### Create a docker network to communicate Spring boot application and MySQL database

3\) Here is the docker command to create a new network:

```docker
docker network create springboot-mysql-net
```

Here springboot-mysql-net is the network name.

4\) Use the below command to list the networks:
```docker
docker network ls
```



#### Run MySQL image in a docker container in the same network

5\) Here is the docker command to run MySQL image in a container in the same network:

```docker
docker run --name mysqldb --network springboot-mysql-net -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=employeedb -d mysql
```


#### Access the MySQL database in a container

5\) Here is the command to access the MySQL database in a container (see more in exercise 1):

```docker
docker exec -it mysqldb bash
```

That's it. Once the MySQL image is deployed in a docker container. Next, we will deploy the Spring boot application in a docker container.

### Spring Boot Application in a Docker Container

 I assume that you have downloaded the Spring boot project from the GitHub repository and set up the project in your IDE.

6\) Next, use the following command to maven build this project:

```maven
mvn clean package
```
Once maven builds success, go target folder and you will be able to see the
springboot-services-0.0.1-SNAPSHOT.jar file generated under the target folder.

#### Create Dockerfile to Build the docker image
Docker builds images automatically by reading the instructions from a Dockerfile. The Dockerfile is a text file that contains all commands, in order, needed to build a given image. 

Let's go to the project root directory and create a file named Dockerfile and the following content to it:


```docker
FROM eclipse-temurin:17

WORKDIR /app

COPY target/springboot-services-0.0.1-SNAPSHOT.jar /app/springboot-services.jar

ENTRYPOINT ["java", "-jar", "springboot-services.jar"]
```

FROM: A docker image can use another image available in the docker registry as its base or parent image. In the above example, we use the eclipse-temurin:17 image as our base image.

WORKDIR: This instruction creates a working directory in a docker container.

COPY: The COPY instruction copies new files or directories and adds them to the filesystem of the container at the path.

ENTRYPOINT: This is where you configure how the application is executed inside the container.

#### Adding Profile to Deploy in Docker Environment
Let's implement the profile in the  Spring boot  application to deploy it in a docker environment.

Let's create an application-docker.properties file under the resources folder and the below property to active docker profile in the application.properties file:

```docker
spring.profiles.active=docker
```

Next, let's change the application-docker.properties file as per the docker environment:

```docker
spring.datasource.url=jdbc:mysql://mysqldb:3306/employeedb
spring.datasource.username=root
spring.datasource.password=root

spring.jpa.hibernate.ddl-auto=update
```

The localhost won't work in the docker network so we change it to the container name which is 'mysqldb'.


```docker
spring.datasource.url=jdbc:mysql://mysqldb:3306/employeedb
```


Next, use the following command to maven build this project:

```maven
mvn clean package
```

Once maven builds with success, go to the target folder and you will to see the
spring-boot-restful-web services-0.0.1-SNAPSHOT.jar file.


#### Build Docker Image from Dockerfile

Now that we have defined the Dockerfile, let’s build a docker image for our application.

Make sure that you’ve packaged the application in a jar file. 

Let’s now build the docker image by typing the following command:

```docker
docker build -t springboot-services .
```

The file path . defines the location of the Dockerfile in the current directory, and the -t argument tags the resulting image, where the image name is the springboot-restful-webservices and the tag is the latest.

After the build is successfully finished, we can check to see if it appears in the list of docker images available locally. To do so, we can execute the below command.


```docker
docker images
```

#### Run a docker image in a docker container in the same network

Once you have a docker image, you can run a container like this:


```docker
docker run --network springboot-mysql-net --name springboot-mysql-container -p 8080:8080 springboot-services
```

Note that we are running the Spring boot application in a container in the same docker network (springboot-mysql-net).

### Demo

Test CRUD services using Postman Client or IntelliJs http client.

Create User REST API:
Request URL: http://localhost:8080/api/users
HTTP Method: POST
Request Body:

```json
{
    "firstName": "ramesh",
    "lastName":"fadatare",
    "email": "ramesh@gmail.com"
}```

Get User REST API:
Request URL: http://localhost:8080/api/users/1
