This section is going to cover the FROM and RUN instructions. 

Every dockerfile must start with the FROM instruction. This is the starting point of our container and is called the base image. 
You can use any image available on the docker registry. To find a list of available images go to https://hub.docker.com. When first starting
out with Docker users are encouraged to use the Official Images in their projects. The reason behind this is there is clear documenation,
they are maintained, security updates are applied in a timely manner and they come from a trusted source.

The RUN instruction will execute any commands in a new layer on top of the current image and commit the changes. 

The Dockerfile1 directory contains a simple Dockerfile that install's vim and curl inside a ubuntu:18.10 base image.

To build the image run:
    ``` $ docker build -t firstcontainer . ```

    Breakdown of command:
	- docker build -- Build an image from a dockerfile
	- -t -- Name/tag your image
	- . -- Instructs docker to use the Dockerfile in the current directory

You can check that your image was built by running: 
    ``` $ docker images ```

To run the container:
    ``` $ docker run --rm -it firstcontainer:latest /bin/bash ```

    Breakdown of command:
	- docker run -- Run a command in a new container
	- --rm -- Automatically remove the container when it exits
	- -it -- Interacive terminal
	- firstcontainer:latest -- Name of image to use
	- /bin/bash -- Command to execute when container starts

Check that vim was installed by creating a file. 

Note: that you can combine instructions into a single layer. See the example Dockerfile in the combine-statements directory
