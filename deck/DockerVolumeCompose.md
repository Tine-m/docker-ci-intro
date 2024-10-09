# Exercise 3: Docker Volumes \+ Docker-compose for MySQL             

[Docker Cheat Sheet](Docker-Cheat-Sheet.pdf): Commands that might come in handy for the exercises.

### Learning outcome
<li>Understand and use the concept Docker Volumes</li>
<li>Know how to setup a MySQL Docker container where data will  "survive"  being closed down and restarted</li>
<li>Understand how to define and run a container using docker-compose</li>


# 1 Docker Volumes

In exercise 1 you learned how to start a container running a MySQL server.  
The problem with the guide you followed was that you lost all your data when the container was stopped. This exercise will solve this problem.

If you have a mySQL container running (docker ps) or stopped (`docker ps -a`),  stop and remove it before you continue with the following steps.

### First repeat exactly what you did in exercise 1 to recap the problem we are trying to solve (persisting data)

**a**) `docker run --name docker-mysql --restart unless-stopped -p 3307:3306 -e MYSQL_ROOT_PASSWORD=test-1234 -d mysql:8.0.38`  
Everything above in ONE LINE.

**b)** `docker exec -it docker-mysql bash`  →  This opens a terminal connected to the bash shell running INSIDE your container. 

**c)** In this terminal type the following to open a MySQL Client: 

`mysql -u root -p`   →  Enter the password you selected when you started the container (test-1234, if you did CUT AND PASTE)

**d)** Try and execute a few SQL commands, to convince yourself, that we have a functional MySQL-client, for example:	  
```sql
show databases;  
create database dummy_db;
show databases; --Verify that dummy_db was create
```

Now type **exit** to return to you  bash terminal in the container and type **exit** one more time to return to "your own" terminal

Now stop and remove the container docker `rm docker-mysql -f`   (-f force to remove even if the container exists)  
Repeat a-c from above, but in d) only run `show databases;`    
This should verify that you have lost the database created in the previous container

### MySQL running in a container with a Docker Volume

1. Now stop and remove the container one more time and enter the following commands:  
     
2. `docker volume ls`  To see your volumes (probably none, unless you have created volumes while experimenting)  
     
   Everything below in ONE LINE:
     
4. `docker run --name docker-mysql --restart unless-stopped -v DATA:/var/lib/mysql -p 3307:3306 -e MYSQL_ROOT_PASSWORD=test-1234 -d mysql:8.0.38`  
6. Repeat step **b-d** from above and **stop** and **close** the container.  
7. Now start the container again, and repeat step b+c.   
8. In step d, just verify that the database has survived that the container was when restarted with the DATA volume

---

# 2 Starting a MySQL container using Docker Compose

In this part we will use docker compose to start our single container.  
We will use the Bind Mount strategy for the volume, so we can see the actual folder that holds the database for the volume. 

Even though we only have one container, using Docker Compose can help us set up the environment, and spare us the problem with copying long docker commands and keep them as one line.  

Create an empty directory somewhere on you computer and add these two files with the given content (spelled exactly like this) into the directory:

**docker-compose.yml**
```docker
version: "3.8"
services:
  db:
    container_name: mysql_db
    image: mysql:8.0.38
    restart: unless-stopped
    env_file: .env
    environment: 
      - MYSQL_ROOT_PASSWORD=$MYSQL_ROOT_PASSWORD
      - MYSQL_DATABASE=$MYSQL_DATABASE
    ports:
      - $MYSQLDB_LOCAL_PORT:3306
    volumes:
       - ./data:/var/lib/mysql # for data
volumes:
  DATA:
```

**.env**  
```docker
#Never publish this file on a public repo and use SAFE PASSWORDS for production
MYSQL_ROOT_PASSWORD=test12
MYSQL_DATABASE=test_db  
MYSQLDB_LOCAL_PORT=3307
```

Verify that your directory only contains the two files introduced above and do the following:

1. Run   `docker compose up -d` to start the container

2. Repeat step a-d (don't delete the database you create in step d) and exit the mySQL Client and your container (exit \- exit)

3. Stop and remove the container like this: `docker compose down` 

4. Verify, in the directory with your two files, that a directory **data** has been created with your actual database (take my word for that)

Now repeat step 1-3, and in step 2, verify that the database copied into the data folder is mounted into the container, so we can start and stop the container without losing data.  

| [Prev <<](./DockerWebApp.md) | [>> Next](./DockerWebAppMySQL.md) | 
|:------:|:------:|
