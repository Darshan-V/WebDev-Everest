 ==Docker is a software platform that allows you to build, test, and deploy applications quickly. Docker packages software into standardised units called containers that have everything the software needs to run including libraries, system tools, code, and runtime.== 


#### Container
```
A Docker container is like a tiny, magical computer inside your computer. It has everything the program needs to run, like all the pieces of a puzzle. But instead of being spread out on a table, they're all neatly packed inside the container. This means the program can be put into the container and run smoothly, without needing to change anything on the computer it's running on.

So, if you have a favorite game or app and you want to share it with your friends, you can put it in a Docker container, just like putting it in your magic box, and they can run it on their computers too, just like you do on yours!
```

#### why use it ? reasons to drive developers and business people to use docker ?
![[Pasted image 20240502160713.png]]
- Reproducibility
- Efficiency
- Scalability
- Portability
- Developer Productivity

#### Dockerfile
==Dockerfile serves as a blueprint for creating a custom docker image based on a specific base image==

#### Docker Image
==Docker image consists of multiple layers, each representing of specific step or instruction defined in the docker file==

#### Docker container
==Docker container can be scaled horizontally to handle increased load or perform distributed process==

![[Pasted image 20240502161327.png]]


![[Pasted image 20240502161337.png]]

#### Essential docker commands
- **docker search:** Finds Docker images on Docker Hub.
- **docker pull:** Downloads an image from a registry to your local machine.
- **docker images:** Lists Docker images on your machine.
- **docker build:** Builds a Docker image from a Dockerfile.
- **docker run:** Creates and starts a new container from a Docker image.
- **docker ps:** Shows running containers; use the `-a` flag to see all containers.
- **docker exec:** Executes a command inside a running container.
- **docker logs:** Displays the logs of a container.
- **docker stop:** Stops a running container.
- **docker rm:** Removes the container
- **docker rmi:** Remove the docker image

### How I did it
- First created a server file with responses with a message
- Created a docker file 
	- specified node version ( since my apis runs on node )
	- specify a working directory in the docker container
	- specify what and where to copy 
	- add a install command **(reason is to not copy every thing from the project while building, ignore all the node_modules and not necessary files in the dockerignore, so the when run install in the docker container installs all the required packages and libraries from the apps package json)**.
	- command to start the app

#### Docker postgres
(Need docker desktop)

```console
docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -p 5432:5432 -d postgres
```

What i'm doing
```
docker run --name shoppingList-postgres -e POSTGRES_PASSWORD=shoppinglist -p 5432:5432 -d postgres
```
