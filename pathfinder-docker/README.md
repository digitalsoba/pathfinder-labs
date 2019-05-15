# Pathfinder - Building Docker containers
This repo contains the labs for META+LAB's operations pathfinder program. This focuses on building Docker images from the ground and running these as containers. 

## Docker images
A docker image is built from a series of layers. These layers contain binaries and instructions that create an image. Think of layers as ingredients of a recipe, and the image is the final dish. We can grab images from official docker hub repository https://hub.docker.com/. A docker image repository is a marketplace where we can base images off one another. We can save images to our local machine by pulling the image with `docker pull image:tag` for example. `docker pull ubuntu:latest`

**Note** we pulled an ubuntu image with the latest tag. A tag represents the version of a certain docker image. 

## Building images 
We build images from a `Dockerfile`. This will contain the steps and layer for the final image. The sample below installs an apache2 image from a ubuntu base. 

```Dockerfile
# This is the base image
FROM ubuntu:latest

# This is the run layer to update and install apache2
RUN apt update \
  && apt install apache2 -y

#Set environment variables for apache
ENV APACHE_RUN_USER=www-data \
  APACHE_RUN_GROUP=www-data \
  APACHE_LOG_DIR=/var/log/apache2 \
  APACHE_PID_FILE=/var/run/apache2.pid \
  APACHE_RUN_DIR=/var/run/apache2 \
  APACHE_LOCK_DIR=/var/lock/apache2

#Create directories
RUN mkdir -p $APACHE_RUN_DIR $APACHE_LOCK_DIR $APACHE_LOG_DIR

# Run apache service
CMD ["apache2", "-D", "FOREGROUND"]
```

From here, we can build the image with `docker build -t pathfinder .` We use the `-t` flag to tag our images, if we don't add a tag docker not add one (It'll be harder to reference)

We can now see our image using `docker images` Let's run our image we just create!

`docker run -p 80:80 pathfinder:latest` - this run our docker image and maps port to 80. We can visit localhost to see our running container

## Useful CLI commands
`docker pull ubuntu:latest` - This pulls the latest ubuntu docker image from a registry

`docker build -t tag .` - Builds an image and creates a tag

`docker images` - List all docker images

`docker ps` - Show running containers

`docker ps -a` - Show running + stopped containers

## Labs
These labs will use the `ubuntu:latest` image
### Lab 1
* `FROM ubuntu:latest`
* Update image
  * `apt update`
* Install sudo
  * `apt install -y sudo`
* Add docker user 
  * `adduser --disabled-password --gecos '' docker`
* Add docker user to sudo 
  * `usermod -aG sudo docker`
* Add user to sudoers file
  * `echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers`
* Build the image and tag it pathfinder:lab1

### Lab 2
* Take the same Dockerfile from lab 1, but add the apache2 package. 
* Expose port 80
* Add the following lines to the dockerfile
```Dockerfile
#Set environment variables for apache
ENV APACHE_RUN_USER=www-data \
  APACHE_RUN_GROUP=www-data \
  APACHE_LOG_DIR=/var/log/apache2 \
  APACHE_PID_FILE=/var/run/apache2.pid \
  APACHE_RUN_DIR=/var/run/apache2 \
  APACHE_LOCK_DIR=/var/lock/apache2

#Create directories
RUN mkdir -p $APACHE_RUN_DIR $APACHE_LOCK_DIR $APACHE_LOG_DIR
```
* Add `CMD ["apache2", "-D", "FOREGROUND"]` to end of the `Dockerfile`
* Build and tag this image `pathfinder:lab2` 
* Run the image and map port 80 to 80
* Visit localhost

### Lab 3
* Build off the second lab
* `COPY` or `ADD` the index.html file into /var/www/html/index.html
* Build and tag this image `pathfinder:lab3` 
* Run the image and map port 80 to 80
* Visit localhost
