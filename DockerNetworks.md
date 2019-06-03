# Introduction
Docker network allowing containers to communicate each others.

Docker has two approaches to networking. The first defines a link between two containers. This link updates /etc/hosts and environment variables to allow containers to discover and communicate.

The alternate approach is to create a docker network that containers are connected to. The network has similar attributes to a physical network, allowing containers to come and go more freely than when using links.
# Working with Docker Network
## Create and connect to a Network
The first step is to create a network using the CLI. This network will allow us to attach multiple containers which will be able to discover each other.

In this example, we're going to start by creating a backend-network. All containers attached to our backend will be on this network.
### Create Network
To start with we create the network with our predefined name. 

`docker network create backend-network`

### Connect To Network
When we launch new containers, we can use the `--net` attribute to assign which network they should be connected to. 

`docker run -d --name=redis --net=backend-network redis`

## Network Communication
Unlike using links, docker network behave like traditional networks where nodes can be attached/detached.

### Explore
Containers can communicate via an Embedded DNS Server in Docker. This DNS server is assigned to all containers via the IP 127.0.0.11 and set in the resolv.conf file.

`docker run --net=backend-network alpine cat /etc/resolv.conf`

Output of above command:
```
nameserver 127.0.0.11
options ndots:0
```

When containers attempt to access other containers via a well-known name, such as Redis, the DNS server will return the IP address of the correct Container. In this case, the fully qualified name of Redis will be redis.backend-network.

`docker run --net=backend-network alpine ping -c1 redis`

Output of above command:
```
PING redis (172.19.0.2): 56 data bytes
64 bytes from 172.19.0.2: seq=0 ttl=64 time=0.099 ms

--- redis ping statistics ---
1 packets transmitted, 1 packets received, 0% packet loss
round-trip min/avg/max = 0.099/0.099/0.099 ms
```

## Connect Two Containers
Docker supports multiple networks and containers being attached to more than one network at a time.

For example, let's create a separate network with a Node.js application that communicates with our existing Redis instance.
### Create a new second network:

`docker network create frontend-network`

### Attach existing "redis" container to second network:
Using the connect command it is possible to attach existing containers to the network.

`docker network connect frontend-network redis`

### Communicate web server to Redis instance
When we launch the web server, given it's attached to the same network it will be able to communicate with our Redis instance.

`docker run -d -p 3000:3000 --net=frontend-network katacoda/redis-node-docker-example`

Test it using `curl localhost:3000`

## Create Aliases

Links are still supported when using docker network and provide a way to define an Alias to the container name. This will give the container an extra DNS entry name and way to be discovered. When using --link the embedded DNS will guarantee that localised lookup result only on that container where the --link is used.

The other approach is to provide an alias when connecting a container to a network.

### Connect Container with Alias
The following command will connect our Redis instance to the frontend-network with the alias of db.

`docker network create frontend-network2`

`docker network connect --alias db frontend-network2 redis`

When containers attempt to access a service via the name db, they will be given the IP address of our Redis container.

`docker run --net=frontend-network2 alpine ping -c1 db`

## Disconnect Containers
With our networks created, we can use the CLI to explore the details.

### List all the networks on our host.

`docker network ls`

### Explore the network to see which containers are attached and their IP addresses.

`docker network inspect frontend-network`

### Disconnects the  container from the network

`docker network disconnect frontend-network redis`
