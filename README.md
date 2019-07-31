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

## Enable internet access on docker container:
### Update deamon.json file
\[/etc/docker/daemon.json\]
```
{
    "dns": ["4.4.4.4", "8.8.8.8"]
}
```
### Restart docker service:
``` service restart docker ```
# Useful commands:
## Remove all stopped containers:
``` docker container prune ```
## Stop all containers
``` docker container stop $(docker container ls -aq) ```
## Remove all containers
``` docker container rm $(docker container ls -aq) ```
