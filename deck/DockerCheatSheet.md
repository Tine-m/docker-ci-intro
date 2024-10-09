# Docker Cheat Sheet

You will find most of the commands you need in this document. Just below the table there's a link to a more complete Cheat Sheet.

| docker pull mysql:latest | Pulls an image from the Docker Registry |
| ----- | :---- |
| docker **images** | List all images |
| docker inspect name OR id | Info about a container |
| Start and run a container |  |
| docker **run** \--name a\_name IMAGE example: docker run \--name a\_name \--env MYSQL\_ROOT\_PASSWORD=secret \--detach mysql:8.0.28 Some common flags: \-d \--deteach → run detached  (will not block the terminal) \--rm         → Remove container once it stops  \-p \--publish → Publish a containers port(s) to the host |  |
| Manage Containers |  |
| docker **ps**   docker **ps \--all** | List all running containers List all containers, including stopped containers |
| docker **stop** a\_name docker **stop** e4a… | Stop running container using its name Stop a running container using its id. You usually only need the first 3-4 character of the id |
| docker **restart** a\_name docker **restart** e4a… | Restart container using its name Restart a container using its id |
| docker **exec** \-it a\_name bash | Access the running container in an interactive terminal (-it) and get a bash shell in the container |
| Logging/debug |  |
| docker **logs** a\_name | Fetches the logs from the specified container. A must, if something does not work as expected |
| docker **logs** \-follow a\_name | Gets loginformation (debug statements) live |
| Clean up |  |
| docker **rm** a\_name docker **rm \-f** a\_name | Removes a stopped container Removes a container, even if running (-f \= force) |
| docker **rmi** f1aa.. | Removes an image *After some time with docker you should use docker images to see whether you have unused images, and the space they take up*. |
| docker **system prune** | Remove all **unused** Docker Objects  (use with care) *Do this from time to time, after having played around with docker to free up resources* |
| docker **system df** | List used space |

You can find a more complete Cheat Sheet here: [https://dockerlabs.collabnix.com/docker/cheatsheet/](https://dockerlabs.collabnix.com/docker/cheatsheet/) 

# Docker Compose Cheat Sheet

The following is probably the only commands you will need for docker-compose this semester

Important: Remember on your vm you call docker compose like this  **docker compose** not **docker-compose** (no hyphen between the two words)

All commands below must be executed in the root of the project (where docker-compose.yml) is located

| docker-compose up \--build \-d | Build and start containers defined in a docker-compose.yml file |
| :---- | :---- |
| docker compose stop docker ps \-a  | Stop the containers Verify that the containers was stopped |
| docker compose start docker ps  | Restart the containers Verify that the containers is running |

