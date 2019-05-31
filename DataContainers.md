# Introduction:
Data Containers are containers whose sole responsibility is to be a place to store/manage data.
Like other containers they are managed by the host system. However, they don't run when you perform a docker ps command.
# Working with Data Containers:
## Create Container:
When creating the container, we also provide a -v option to define where other containers will be reading/saving da
`docker create -v /config --name dataContainer busybox`
## Copy Files to Data Container:
`docker cp config.conf dataContainer:/config/`
## Mount Volumes From
`docker run --volumes-from dataContainer ubuntu ls /config`

If a /config directory already existed then, the volumes-from would override and be the directory used. You can map multiple volumes to a container.
## Export / Import Containers
If we wanted to move the Data Container to another machine, we can export it to a .tar file and then import the Data Container back into Docker.
`docker export dataContainer > dataContainer.tar`

`docker import dataContainer.tar`
