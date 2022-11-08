# Docker Cheat Sheet

- Cheat sheet for docker

## Docker Commands

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
- We have `-d` flag that runs the container at background
- We also have `-p` flag that maps the port of local machine and container eg `<local-machine-port>:<container-port>`
- We also have `-v` flag to create volume to reference local files on the container to watch the local changes
    - **eg**: `docker run -p 3000:3000 -v /app/node_modules -v $(pwd):/app <image_id>`
        - Here first `-v` flag with node_modules puts a bookmark on the node_modules folder 
        - second `-v` flag maps the present working directory to the `/app` directory of the container
        
> Note: this creates new container so only use this command when wanting the new container from the image
> 
> We can use `start` command to restart the container

<br>

### `docker images`

- This command shows list of images present on a system

<br>

### `docker image rm <image-name>`

- Removes specified image, we can add `-f` flag to force delete the image

<br>

### `docker ps`

- This command shows the list of running containers on the system
- We can use `-a` flag with this to see all the unactive containers on the system

<br>

### `docker container rm <container-id>`

- This command removes the specific container

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

### `Dockerfile`

- This is a file format which specifies the steps to take when building a image form the list of instructions provided on it.
- Example:

```Dockerfile

FROM node:17-alpine # imports node version 17 on alpine linux

RUN npm install -g nodemon # installs nodemon as global dependency on specified platform above

WORKDIR /app # specifies working directory on a container

COPY package.json . # copies package.json to that particular directory on container

RUN npm install # installs all other dependencies present in package.json

COPY . . # copies all file form working directory to container directory i.e. /app

EXPOSE 4000 # exposes port of a container

CMD ["npm", "run", "dev"] # start the script from package.json

```

### `docker build <Dockerfile's relative locaion>`

- This command builds a image from a `Dockerfile` contents this gives us image id from that we can create our custom container
- We can use `-t` flag to name the image, the standard patern is `-t <docker-id>/<project-name>:<version>`
- We can use `-f` flag to speficy the custom Dockerfile like `Dockerfile.dev`
- cont......

<br>

## Docker-Compose commands

**`docker-compose.yml` file**

```yml
version: '3.8'
services:
  redis-server:          
    image: 'redis'
  node-app:                 # Service name
    restart: always         # Restart Policies ( more on this below )
    build: .                # Dockerfile's relative location ( but if our dockerfile is like Dockerfile.dev then we change some things on thi )
    ports:
      - "5050:8080"         # Local-port : Container-port
    volumes:                # Volumes are the preferred mechanism for persisting data generated by and used by Docker containers 
      - /app/node_modules   # Puts a bookmark on the node_modules folder
      - .:/app              # Maps the present working directory to the `/app` directory of the container
```

**Restart Policies**

- `"no"`            : Never try to restart this container when it crashes
- `always`          : Always attempt to restart container if it stops for any reason
- `on-failure`      : Only restart container if it stops with a error code
- `unless-stopped`  : Always restart unless we forcibly stop it

> Note: We have only quoted `"no"` and not others beacause in a yaml file the value `no` is interpreted as false

**For `Dockerfile.dev` we do this insted of the one above**

```yml
    build:
      context: .
      dockerfile: Dockerfile.dev 
```


### `docker-compose up`

- This command is similar to `docker run <image-name>`. It runs according to config defined at `docker-compose.yml` file
- Contains `--build` flag which rebuilds the images defined inside of `docker-compose.yml` file. This is same as running both `docker build .` and `docker run <image-name>`
- We use `-d` flag to launch containers on background

> Note: We can skip `-` and do `docker compose up` and it works fine

<br>

### `docker-compose ps`

- This command prints out the status of the containers running inside of the `docker-compose` file

> Note:  When should run this command from same directory as our `docker-compose.yml` file as when we run this command first it tries to find `docker-compose.yml` file on the local directory and it tries to find all the running containers on our local machine that belong to that specific `docker-compose.yml` file

### `docker-compose down`

- This command stops running docker-compose.yml 

<br>

### `docker-compose down --rmi all -v`

- This command stops docker-compose.yml and deletes all Image, Container and Volume

<br>
