
## Installing docker on ubuntu :

### 1. install docker package :

	$ sudo apt install docker.io


### 2. enable docker service :

	$ sudo systemctl enable docker
	
### 3. Chekc the docker service status :

	$ sudo systemctl status docker
	
### 4. Check Docker version :

	$ docker --version
	
Sample Output :
	
	Docker version 24.0.5, build 24.0.5-0ubuntu1~22.04.1

### 5. Run sample hello-world docker image :

	$ sudo docker run hello-world

sample output :

	Hello from Docker!
	This message shows that your installation appears to be working correctly.

	To generate this message, Docker took the following steps:
	1. The Docker client contacted the Docker daemon.
	2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
		(amd64)
	3. The Docker daemon created a new container from that image which runs the
		executable that produces the output you are currently reading.
	4. The Docker daemon streamed that output to the Docker client, which sent it
		to your terminal.

	To try something more ambitious, you can run an Ubuntu container with:
	$ docker run -it ubuntu bash

	Share images, automate workflows, and more with a free Docker ID:
	https://hub.docker.com/

	For more examples and ideas, visit:
	https://docs.docker.com/get-started/



