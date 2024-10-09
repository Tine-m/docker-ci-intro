# Exercise 5: Docker Compose: multi container application

With Compose, we use a YAML file to configure our application’s services. Then, with a single command `docker-compose up`, we create and start all the services from our configuration.

Compose works in all environments: production, staging, development, testing, as well as CI workflows. It also has commands for managing the whole lifecycle of your application: 

Start, stop, and rebuild services 
View the status of running services 
Stream the log output of running services 
Run a one-off command on a service

### Preconditions

This assumes you have installed Docker and Maven locally on your developer computer.

Make a new clone of the previous Spring Boot Project.

### Learning outcome

* BUser Docker compose to run web app and database together in network with docker compose

### Creating the docker-compose.yml configuration


1\)  
For a Spring Boot MySQL application using docker-compose, we need to create a docker-compose.yml file that contains the configuration for all the application services.

Go to the root directory of our project, and create a docker-compose.yml file:

Next, let's add the below configuration to a docker-compose.yml file (you could consider using and .env configuration file as in exercise 3 about Docker-Compose):
```docker
services:
  mysqldb:
    container_name: mysqldb
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: employeedb
    networks:
      springboot-mysql-net:

  springboot-services:
    container_name: springboot-services
    build:
      context: ./
      dockerfile: Dockerfile
    ports:
      - "8080:8080"
    depends_on:
      - mysqldb
    networks:
      springboot-mysql-net:
    restart: on-failure

networks:
  springboot-mysql-net:
```

Let's understand the above docker-compose.yml file.

We have configured two services:

**mysqldb service**: It runs MySQL database that runs on port 3306
**springboot-services service**: Deploy Spring boot application in tomcat server on port 8080

**build** - Specify the location of the Dockerfile.

**depends_on** - Notice the depends_on keyword in the compose file. Docker compose will make sure that all the dependencies of a service are started first before starting the service itself.

**environment** – In this section, we are setting the environment variables for MySQL root password and MySQL database name.

**ports** – Here we are mapping the Host port with the port inside a docker container.

**restart**: on failure - Restart the container if it stops on failure. 


### Running Spring Boot Application and MySQL Database Using Docker Compose
2\)  
Let's use below single docker-compose command to run the whole setup:

```docker
docker-compose up
```

3\)  
Use this below docker-compose command to start and run all the containers in the background:

```docker
docker-compose up -d
```

4\)  
Use this docker command to check the list of currently running containers:

```docker
docker ps
```

5\)  
Use this docker command to list all the docker images:

```docker
docker images
```

### Test CRUD RESTful services using Postman Client or HTTP client in Intellij
6\)  
Create User REST API:
Request URL: http://localhost:8080/api/users
HTTP Method: POST
Request Body:

```json
{
    "id": 1,
    "firstName": "Santa",
    "lastName":"Claus",
    "email": "xmas@gmail.com"
}
```

| [Prev <<](./DockerWebAppMySQL.md) |   | 
|:------:|:------:|

