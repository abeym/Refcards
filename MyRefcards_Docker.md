# Docker on Windows

### Purpose
To install and user Docker on windows.

## Study Link
Ref:https://docs.docker.com/engine/getstarted/step_one/

## Downlaod

If Hyper-V is active on windows use : [https://docs.docker.com/docker-for-windows/](https://docs.docker.com/docker-for-windows/)

If using Windows without Hyper-V use : [Docker Toolbox](https://docs.docker.com/toolbox/overview/)

## Install

I instaled Docker Toolbox
Open docker terminal and verify installation 

Check that you have the latest release

	docker version
	docker info
	

## Config

## Usage 

	Go to Programs > Docker > Docker > Docker Quick Start Terminal
	Or Programs > Docker > Docker > Kitematic > Teminal
	
###### Start / Stop
	docker-machine ls
	docker-machine stop default
	docker-machine ls
	docker-machine start default
	
##### Getting started

See if you have any running containers

	docker ps
	docker ps -a

Download and run hell-world

	docker run hello-world

run an Ubuntu container in a Bash shell

	docker run -it ubuntu bash
	exit

start a Dockerized webserver. 

	docker run -d -p 80:80 --name webserver nginx
	
Stop and start web server 

	docker ps -a	
	docker stop webserver
	docker start webserver

Point your web browser at http://192.168.99.100/ to display the start page.	
To find the ip address of docker 

	docker-machine ip

(Note: Another method is in Virtualbox click File>Preferences>Network>Host-only Network and hover over `VirtualBox Host-Only Ethernet Adapter #4` . Look for ip of  `Lower Bound`)

Register on [Docker Hub signup page](https://hub.docker.com/register/).
Confirm Your Email. Create Repository. Set the repo Visibility is set to Public.
Build your own image 

	docker run docker-whale
	docker images
	
	cd /c/Abey/Projects/docker/mydockerbuild
	touch Dockerfile
	vi Dockerfile

		FROM docker/whalesay:latest
		RUN apt-get -y update && apt-get install -y fortunes
		CMD /usr/games/fortune -a | cowsay

	cat Dockerfile
	docker build -t docker-whale .

Tag and push

	docker login
	docker tag 1429198da1b4 abeym/docker-whale:latest
	docker push abeym/docker-whale
	docker images

Remove and pull 

	docker rmi -f 1429198da1b4
	docker rmi -f abeym/docker-whale
	docker rmi -f docker-whale
	docker images
	docker run abeym/docker-whale	

##### Build and manage Machine, Container, Image, Application
 Ref : [Docker Tutorials](https://docs.docker.com/engine/tutorials/) 



## Projects

## Tips & Notes

### Help


### Version


### Command 1

    command 1

### Command 2

    command 2
    
