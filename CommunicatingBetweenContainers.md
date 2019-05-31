# Introduction
This scenario explores how to allow multiple containers to communicate with each other. The steps will explain how to connect a data-store, in this case, Redis, to an application running in a separate container.
# Communicating Between Containers
The most common scenario for connecting to containers is an application connecting to a data-store. The key aspect when creating a link is the name of the container. All containers have names, but to make it easier when working with links, it's important to define a friendly name of the source container which you're connecting to.
## Start Data Store
Run a redis server with a friendly name of redis-server which we'll connect to in the next step. This will be our source container.

`docker run -d --name redis-server redis`
## Create Link
To connect to a source container you use the --link <container-name|id>:<alias> option when launching a new container. The container name refers to the source container we defined in the previous step while the alias defines the friendly name of the host.

By setting an alias we separate how our application is configured to how the infrastructure is called. This means the application configuration doesn't need to change as it's connected to other environments.
### How Links Work
First, Docker will set some environment variables based on the linked to the container. These environment variables give you a way to reference information such as Ports and IP addresses via known names.

You can output all the environment variables with the env command. For example:

`docker run --link redis-server:redis alpine env`

Secondly, Docker will update the HOSTS file of the container with an entry for our source container with three names, the original, the alias and the hash-id. You can output the containers host entry using `cat /etc/hosts`

`docker run --link redis-server:redis alpine cat /etc/hosts`

With a link created you can ping the source container in the same way as if it were a server running in your network.

`docker run --link redis-server:redis alpine ping -c 1 redis`
