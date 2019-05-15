Before we get started, let's cover the FROM and RUN instructions. 

Every dockerfile must start with the FROM instruction. This is the starting point of our container and is called the base image. 
You can use any image available on the docker registry. To find a list of available images go to https://hub.docker.com. When first starting
out with Docker users are encouraged to use the Official Images in their projects. The reason behind this is there is clear documenation,
they are maintained, security updates are applied in a timely manner and they come from a trusted source.

The RUN instruction will execute any commands in a new layer on top of the current image and commit the changes. 

Now let's create a simple dockerfile and use the ubuntu:latest as are base image.
    - Create a directory called first-dockerfile
    - Create and open a file named Dockerfile inside the first-dockerfile directory

    The contents of the file: 
        FROM ubuntu:latest
        RUN apt-get update
        RUN apt-get -y install vim

We now have a complete dockerfile and it is time to build our container. To do that we use the following command:
    ``` $ docker build -t firstcontainer . ```
    Breakdown of command:
    - docker build -- Build an image from a dockerfile
    -t -- Name/tag your image 
    - . -- Instructs docker to use the Dockerfile in the current directory

You can check that your image was built by running: 
    ``` $ docker images ```


