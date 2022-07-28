# Docker

## What is Docker?
Docker allows for the creation of containers of code, that contain all dependencies and information that allow that code to function. The containers can be easily moved around and run. 

containers live in a container repository, many companies have private repositories. However, most are publically stored on DockerHub

https://hub.docker.com/

before contianers?

you must install all the services on your own computer. Need postgres sql? must download it yourself. But with docker, all of these are packaged inside the package as needed. With more installations, the more something can go wrong!

## Deployment
before docker:
jar file, a database service, all separate and with their own instructions
you need an operations team to set up the enviornment for the applications. 

Everything must be iunstalled directly on the server and operating system

what if the devs forget to mention something about config? Eveyrhting goes to SHIT

after docker:
with containers, the developers and operators are working in one team, so all the dependencies are encapsulated in one single environment. None of the stuff has to be configured directly on the server. You only have to run a docker command. You just have to install the docker runtime to run contianers

## what is a container?
Layers of images, mostly linux base images because of their small size (alpine linux)
on top you have the application image, which will run in the contianer

in between is configuration data

## practical example
postgresql
1. download the containers directly from the repository
2. in the command line: docker pull (imagename):(version) 
3. NOTE: to run docker from the command line the desktop app must be open so that the daemon starts

docker ps displays all running containers

Docker image:
The actual application package with the dependencies and everything. 

Docker Container:
When the application starts, it is a container. WHen the image is started it becomes a container, and when it is turned off it goes back to being an image

## Docker vs Virtual Machine
Operating systems have 2 layers, the kernel and the applications. THe kernel unites hardware and application layers, its the connection
docker and virtual machines are both virtualizers

docker virtualizes the application layer. it contains applications layer of an operating system, and uses the kernel of the host 

The virtual machine has applications AND its own kernel

docker images are usually a few gigabytes, virtual machines are a few gigabytes
docker images start up much faster than virtual machines
docker images are operating system specific, virtual machines can run on any operating system. THis is because docker images rely on the host kernel

you can get around this with docker toolbox

containers contain a virtual fgile system. A port is assigned to that container

## Docker COmmands
docker pull:
docker pull (image name)
pulls an image from dockerhub

docker run: does docker pull and start in one command
make a container of the image so it can run
docker run (program)
additionally, you can run docker run -d (process) so that it runs in the background. Returns an ID

docker start:
starts a stopped container
docker start (container id)

docker stop:
stops a docker container. Can be restarted
docker stop (container id)

docker ps:
displays all running containers and their information
docker ps -a shows all containers running or not running

docker exec -it:

docker logs:

docker images:
display all docker images on the machine. All images have tags, or the version

you will always see a port the container is operating on

docker ps -a 

this displays docker run history

## container port
so how do you operate with two containers run on the same port without interfering?

to run the container you must bind the container to a host port on your computer

because each container has its own ports, you can connect different host ports to the same ports on 2 different containers

host        container
port 3000   port 3000
port 3001   port 3000

until we bind a host port to an application port the applications are unusable

docker run -p (desired host port):(known application port) (image name)

## Debugging Containers
what if you wanted to get inside the terminal of a container and execute it directly?

docker logs (container ID)

you can use the --name argument on the run command to add a special name to container

docker exec
get the termninal of a running container

docker exec -it (container ID) /bin/bash 

this will get you ionto the container as a root user. you7 can also use the container name

you can also do 
`docker logs (container name)`

to exit the terminal do exit

## Workflow of Docker

you are developiung a python app on the local environment. YOu need mongoDB

so you download a docker container with mongo DB

now you want to deploy the app on a development environment. You commit the app to git, that will trigger a jenkins build and produce artifacts from the applications. THen you can create a docker container. YOu can push it to a docker repository. Then it can be configured on a development server, which pulls from the private repository. Now you have your app and the mongo DB container running on the server in tandem. Now if another dev logs in, they can test the application

so for our demo we will download a jodejs application and connect that to another container running mongodb

ideally this will make things much easier to do

so how can we use the docker containers to make the development process easier

we will use mongo express,so we can see updates from our application and interact live with our database

### DEocker network
docker makes its isolated docker network where the contianers are deployed. When you deploy two in the same network they can interact just using the container name, because they are in the same network

when we package our app in the network, it will package the mongo db and mongo express inside with it. 

we can see our networks with `docker network ls`

we can make a new one by doing `docker network create`

to run the container in the network we have to give the network name or id

we run the port, with -p, and set the name with --name

we also need to consider environmental variables of the container before we run it

you can specify root username and password, and the initial database, for mongo db as an example

to call the mongodb database we give protocol and the ip:port

## Compose
what if we dont want to execute all those docker commands every time we want to run a container?

we ave a tool that can do it. Docker compose

we use YAML files  to store docker container compositions in them for each container

we do not need to specify networks, because compose takes care of that for us

### docker compose command
docker-compose -f (filename).yaml up 
up starts all the containers in the yaml

note when you reconfigure a container, all the data in that container is LOST! 

you want to have persistence, especially with databases. You will have volumes for that.

to stop a composition of containers:

docker-compose -f mongo.yaml down, stops all containers specified in the file. Also shuts down the network!

## Dockerfile
to deploy an app it needs to be containerized. Now we need to build that docker image. How do we do that? with the dockerfile

we use jenkins to package the image, but we can also do it ourself

so what is the dockerfile?

the dockerfile contains application information and artifacts to configure the application while its being containerized. Its like a high level blueprint, or bill of materials. The syntax of a dockerfile is very simple.

the image always starts with FROM (image) 
where is the image deriving from? This is the base image

we are going to have node installed in our image. When we start our container, we can see that the node command is available.

next we set environment variables with

`ENV (variables)`

this will let us set environment variables from outside the image, so that every time we change something we dont need to rebuild it

then `RUN`

this allows you to run any linux command

`RUN mkdir -p /home/app` : creates a file that will live in the container environment

`COPY . /home/app` : you can copy from your host to the container

`CMD ['node', 'server.js']` : this is an entry point linux command

CMD marks the start of the program to run the server

### docker stack of our app

app:1.0
^
|
node:13-alpine
^
|
alpine:3.10