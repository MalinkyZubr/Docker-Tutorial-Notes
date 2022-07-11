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

docker exec (container ID)