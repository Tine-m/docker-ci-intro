# Exercise 2: Run Web App in a Docker Container

In this exercise you will create a docker image for a Spring boot application, then create a docker container from the image and finally run the app in a docker container.

### Preconditions

This assumes you have installed Docker and Maven locally on your developer computer.

### Learning outcome

* Be able to build a docker image   
* Be able to run image in docker container  
* Be able to run image in docker container in detached mode (background)

### Create/clone a Spring Boot application

1\)  
You can clone this repo as your starting point [springboot-docker-demo](https://github.com/Tine-m/spring-docker-demo)
 or use a similar repo of your own. The repo is a simple REST API with one endpoint:

```java
@RestController  
public class DockerController {

    @GetMapping("/docker")  
    public String dockerDemo(){  
        return "Hello from Spring Boot Application";

    }  
}
```

2\)  
Use the following maven command in the terminal to build the project (a jar-file):

```maven
mvn clean package
```

Once maven builds successfully, go to the target folder and see the springboot-docker-demo-0.0.1-SNAPSHOT.jar (assuming you have cloned my repo).

### Create a Dockerfile

3\)  
Create a file named Dockerfile (spelled exactly like that) in the project root directory and type the following content:

```docker
FROM eclipse-temurin:17

WORKDIR /app

COPY target/springboot-docker-demo-0.0.1\-SNAPSHOT.jar /app/springboot-docker-demo.jar

ENTRYPOINT \["java", "-jar", "springboot-docker-demo.jar"\]
```

**FROM:** A docker image can use another image available in the docker registry as its base or parent image. In the above example, we use the eclipse-temurin:17 image as our base image.

**WORKDIR:** This instruction creates a working directory in a docker container.

**COPY:** The COPY instruction copies new files or directories and adds them to the filesystem of the container at the path.

**ENTRYPOINT:** This is where you configure how the application is executed inside the container.

### Build Docker image

4\)  
Now we have a Dockerfile, let’s build a docker image for our application.
Before building the docker image, make sure that you’ve packaged the application as a jar file using maven. 

You can build the docker image with the following command:

```docker
docker build \-t springboot-docker-demo .
```

The file path . defines the location of the Dockerfile in the current directory, and the \-t argument tags the resulting image, where the **repository** name is the springboot-docker-demo and the tag is the latest.

5\)  
After the build is successfully finished, we can check to see if it appears in the list of docker images available locally. To do so, execute this command:

```docker
docker images
```

### Run Docker image in container

6\)  
Once you have a docker image, you can run it using the docker run command:

```docker
docker run \-p 8080:8080 springboot-docker-demo
```

With the \-p option, we expose the container's 8080 port to the host's 8080\. (The host value is first).

By default, when you run a container using the docker run command, it does not publish any of its ports to the outside world. To make a port available to services outside Docker or to Docker containers that are not connected to the container’s network, use the \--publish or \-p flag.

7\)  
To run the docker container in the background (detached mode), use the \-d option in the docker run command:

```docker
docker run \-d \-p 8080:8080 springboot-docker-demo
```

The above command starts the container in the background and gives you the container ID.

You can see a list of all running containers using the following command:

```docker
docker ls
```

### Demo

8\)  
Once the docker image running in a container. Open the browser and hit this link in the browser [**http://localhost:8080/docker**](http://localhost:8080/docker)  
Hopefully you will see this REST API response message in a browser:

```html
Hello from Spring Boot Application
```

| [Prev <<](./DockerMySQL.md) | [>> Next](./DockerVolumeCompose.md) | 
|:------:|:------:|

