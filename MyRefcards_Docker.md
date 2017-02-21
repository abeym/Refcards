# Docker on Windows

### Purpose
To install and use Docker on windows.

## Study Link
Ref: https://docs.docker.com/engine/getstarted/step_one/

## Downlaod

If Hyper-V is active on windows use : [https://docs.docker.com/docker-for-windows/](https://docs.docker.com/docker-for-windows/)

If using Windows without Hyper-V use : [Docker Toolbox](https://docs.docker.com/toolbox/overview/)

## Install

I instaled Docker Toolbox

Go to Programs > Docker > Docker > Docker Quick Start Terminal  
Or Programs > Docker > Docker > Kitematic > Teminal  
Open docker terminal and verify installation   

Check that you have the latest release

	docker 
	docker version
	docker info

## Config

## Usage 

	
###### Start / Stop
	docker-machine 
	docker-machine ls
	docker-machine stop default
	docker-machine ls
	docker-machine start default
	
#### Getting started

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
Confirm Your Email. Create Repository. Set repo name to docker-whale. Set repo visibility is to Public.

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

	docker images
	docker tag 1429198da1b4 abeym/docker-whale:latest
	docker push abeym/docker-whale
	docker images

Remove and pull 

	docker images
	docker rmi -f 1429198da1b4
	docker rmi -f abeym/docker-whale
	docker rmi -f docker-whale
	docker images
	docker run abeym/docker-whale	
	docker images

Clean up 
  delete all images & containers
  
	docker images -q | xargs docker rmi -f
	docker ps -q | xargs docker rm
	
#### Build and manage Machine, Container, Image, Application
 Ref : [Docker Tutorials](https://docs.docker.com/engine/tutorials/) 

Build your own images

	docker images
	docker search <image / user name>
	docker search sinatra
	docker run -t -i training/sinatra /bin/bash
	# apt-get install -y ruby2.0-dev ruby2.0
	# gem2.0 install json
	# exit
	
	docker images
	docker commit -m "Added json gem" -a "Abey M"  e34d823789cb abeym/sinatra:v1
	docker run -t -i abeym/sinatra:v1 /bin/bash
    
Building an image from a Dockerfile

	cd /c/Abey/Projects/docker/sinatra
	touch Dockerfile
	vi Dockerfile
	
		# This is a comment
		FROM ubuntu:latest
		MAINTAINER Abey M <abey_mail@yahoo.com>
		RUN apt-get update && apt-get install -y ruby2.2 ruby2.2-dev
		RUN gem install json
		RUN gem install sinatra

	docker build -t abeym/sinatra:v2 .
	** dont forget the dot . **
	... Sending build context to Docker daemon 2.048 kB
	...
	...
	... Removing intermediate container 6b81cb6313e5
	.... Successfully built 97feabe5d2ed
	
	docker images
	docker tag 5db5f8471261 abeym/sinatra:v2
	 ** ID of the image, here 5db5f8471261 **
	docker push abeym/sinatra
	docker history 2c6edc1cfab4
	
	docker rmi abeym/sinatra
	docker run -t -i abeym/sinatra /bin/bash
	
#### Connect to running containers
	$ sudo docker ps
		CONTAINER ID  IMAGE            COMMAND    CREATED STATUS  PORTS          NAMES
		665b4a1e17b6  webserver:latest /bin/bash  ...     ...     22/tcp, 80/tcp loving_heisenberg 
	
	$ sudo docker run webserver
	This will start the container
	
	$ sudo docker attach 665b4a1e17b6 #by ID
	or
	$ sudo docker attach loving_heisenberg #by Name
	$ root@665b4a1e17b6:/# 
	
	If we use attach we can use only one instance of shell. So if we want open new terminal with new instance of container's shell, we just need run the following:
	
	$ sudo docker exec -i -t 665b4a1e17b6 /bin/bash #by ID
	or
	$ sudo docker exec -i -t loving_heisenberg /bin/bash #by Name
	$ root@665b4a1e17b6:/#


#### Network containers
	
	docker network ls

bridge is special network and containers are launched by default in it.

	docker run -itd --name=networktest ubuntu
	docker network inspect bridge
	docker network disconnect bridge networktest

  create a network and run a container in it	
	
	docker network create -d bridge my-bridge-network
	docker network ls
	docker network inspect my-bridge-network
	docker run -d --net=my-bridge-network --name db training/postgres
	docker inspect --format='{{json .NetworkSettings.Networks}}'  db
	
  run a container in default network and connect to db.
  
	docker run -d --name web training/webapp python app.py
	docker inspect --format='{{json .NetworkSettings.Networks}}'  web
	docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' web
	docker exec -it db bash
	# ping 172.17.0.2
		^C
		--- 172.17.0.2 ping statistics ---
		44 packets transmitted, 0 received, 100% packet loss, time 43185ms
	# exit
	$ docker network connect my-bridge-network web
	$ docker exec -it db bash
	# ping web
		64 bytes from web (172.18.0.3): icmp_seq=1 ttl=64 time=0.095 ms
		^C
		--- web ping statistics ---
		3 packets transmitted, 3 received, 0% packet loss, time 2000ms

#### Manage data in containers	
	


## Help

### Tips & Notes

##### Error: 
	An error occurred trying to connect: Get http://%2F%2F.%2Fpipe%2Fdocker_engine/v1.24/containers/json: open //./pipe/dock
er_engine: The system cannot find the file specified.
##### Solution:
	Open powershell and run 
	docker-machine env --shell=powershell | Invoke-Expression




	
## Projects

#### QuickStart Option for Apache Hadoop and Cloudera
	Ref: http://blog.cloudera.com/blog/2015/12/docker-is-the-new-quickstart-option-for-apache-hadoop-and-cloudera/
	docker pull cloudera/quickstart:latest
	docker images # note the hash of the image and substitute it below
	docker run --privileged=true --hostname=quickstart.cloudera -t -i -p 80 ${HASH} /usr/bin/docker-quickstart
	# e.g. used this on my machine
	# docker run --privileged=true --hostname=quickstart.cloudera -t -i -p 80 4239cd2958c6 /usr/bin/docker-quickstart
	# When the previous command finishes, it enables a Bash Shell. From this Bash Shell execute the following command in order to see which is the docker image IP: 
	# hostname -I
	# Put this IP in the browser and working!
	


### Version


### Command 1

    command 1

### Command 2

    command 2
    
## Wiki Refs 

1. [Markdown-Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
2. [mastering-markdown](https://guides.github.com/features/mastering-markdown/)

