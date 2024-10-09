# Exercise 5: Docker Compose: multi container application

With Compose, we use a YAML file to configure our application’s services. Then, with a single command, we create and start all the services from our configuration.

Compose works in all environments: production, staging, development, testing, as well as CI workflows. It also has commands for managing the whole lifecycle of your application: 

Start, stop, and rebuild services 
View the status of running services 
Stream the log output of running services 
Run a one-off command on a service

### Preconditions

This assumes you have installed Docker and Maven locally on your developer computer.

### Learning outcome

* Be able to build a Docker image   
* Be able to run image in docker container  
* Be able to run image in docker container in detached mode (background)

### Creating the docker-compose.yml configuration

1\) For a Spring Boot MySQL application using docker-compose, we need to create a docker-compose.yml file that contains the configuration for all the application services.

Go to the root directory of our project, and create a docker-compose.yml file:

Next, let's add the below configuration to a docker-compose.yml file:


