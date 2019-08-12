# MySQL DB
## Create DockerFile:
FROM mysql:latest
## Build Mysql Image:
``` sudo docker build --tag mysql .```
## Start container:
``` sudo docker run -dit  -p 3306:3306 mysql
