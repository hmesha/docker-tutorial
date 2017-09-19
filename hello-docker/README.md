Docker container running centos and prints "Hello Docker..."

To run this tutorial:

1. clone this repository
2. cd to where this repository cloned
3. run the following commands
	1. docker build -t hello-world .
	2. docker run -it --rm --pid=host hello-world
	
The first command above will build a docker image based on centos:latest from the Dockerfile. The second command will run it within the installed docker engine as a container then print "Hello Docker from centos container", remove the container on exit.