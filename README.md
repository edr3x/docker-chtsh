# Docker Cheat Sheet

- Cheat sheet for docker

## Commands

### `docker pull <image-name>`


- This command pulls the particular image from [dockerhub](https://hub.docker.com/)

<br>

### `docker create <image-name>`

- This command creates a new container from the docker image

<br>

### `docker start -a <container-id>`

- This command starts the container `-a` lag makes docker watch for output from the container and print it out on your terminal kind of saying `attach to the container and give me output from that` 
- This restarts the container if the container is closed

> Note: we cannot override the default command which was already on the container

<br>

### `docker run <image-name>`

- This does following things
    - Checks if there is that particular image locally if not then it pulls the image from dockerhub
    - Creates a new container from the image name provided
    - Runs the container with the arguments or commands if provided

> Note: this creates new container so only use this command when wanting the new container from the image
> 
> We can use `start` command to restart the container

<br>

### `docker ps`

- This command shows the list of running containers on the system
- We can use `-a` flag with this to see all the unactive containers on the system

<br>

### `docker logs <container-id>`

- This command is used to look at the container and retrieve all the information that has been emitted from it

> Note: by running this command we are not rerunning or restarting the container in any way shape or form.
> We are just getting record of all the logs that have been emitted from that container.

- This is the really good way to inspect a container and see what is going on inside of it

<br>

### `docker stop <container-id>`

- When this command is issued a hardware signal is sent to the primary process inside that container
- The message `SIGTERM` is sent to the process of the container telling it essentially to shut down the running process on it's own time
- This is used if we want to stop a process and give it little bit of time to shut itself down and do a little bit of cleanup or save some files or emit some message or other things.

### `docker kill <container-id>`

- This command issues a `SIGKILL` command which essentially means the process have to shut down right now and process do not get to do any additional work
- Ideally we use `stop` command but if that doesn't work or we have to kill everything then we use `kill` command

<br>

### `docker system prune`

- This removes all the containers and images from local machine

<br>

### `docker exec -it <container-id> <command>`

- This command is used when we want to execute certain commands inside the particular container
- `-it` allows us to provide input to the container 
    - this is combination of `-i -t` flag in which `-i` means when we execute the command inside the container we want to attach our terminal to std-in channel of that new running process, `-t` flag makes it more readable and provides us more assistance
    - **eg:** We can use `docker exec -it <container-id> sh` to get access to shell on the container and execute command there, and use `ctrl+d` or exec `exit` to exit from that shell

<br>  
