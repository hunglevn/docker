# Basic steps
## Create DockerFile:
DockerFile
FROM debian:latest
## Build image
``` docker build --tag=debian .```
Built image is stored in machine's local Docker image registry. Use this command to show images:
``` docker image ls ```
## Run image:
``` docker run --name debian -dit debian ```
## SSH into a running container
``` docker exec -it <container name> /bin/bash ```

# Useful commands:
## Remove all stopped containers:
``` docker container prune
## Stop all containers
``` docker container stop $(docker container ls -aq)
## Remove all containers
``` docker container rm $(docker container ls -aq)
