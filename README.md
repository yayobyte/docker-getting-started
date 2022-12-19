# GETTING STARTED WITH DOCKER
This repo was generated to start in the `docker` world. Just for learning purposes.
All these docs has been taken from

> docker `run` -d -p 80:80 docker/getting-started

## Prerequisites
- docker installation
- docker desktop
- docker hub account

## Apps
- `webapp` created in react and will use its own container
- `mysql` server in an isolated container

# Contents
## Creating the container
First we have to create a `Dockerfile` in our repo. Make sure you do not use any extension

    FROM node:18-alpine 
    WORKDIR /app
    COPY . .
    RUN yarn install --production
    CMD ["node", "src/index.js"]

> `FROM` which image to use\
> `WORKDIR` which dir will now be used as root from now on inside the container\
> `COPY` from which directory in the code to which `WORKDIR` in the container\
> `RUN` which command to run after previous step\
> `CMD` specifies the instruction that is to be executed when a Docker container starts.


## Building the image
Taking a look at the `Dockerfile` that is the command used to build the image and then run:
> `tag-name` is the name of the app\
> `.` (at the end) as parameter will tell docker to use Dockerfile in the root of the folder\
> \
> docker `build` -t *tag-name* .

![build](/docs/build.png "build")

## Starting a Container
Now that we have an image, we should start the container
> docker `run` -dp 3000:3000 *tag-name*

After a few seconds you will see the webapp running in
> http://localhost:3000

### Implementing changes
You can do any change in the app, and then you can deploy again
**make sure you stop and remove the container first!**

1. Let's list all the containers and to get the `container-id`
> docker `ps`
2. Stop the running container
> docker `stop` *container-id*
3. Remove the container
> docker `rm` *container-id*
4. Build the container again (make sure to use tags to identify your container)
> docker `build` -t *tag-name*
5. Run the container again with the new changes
> docker `run` -dp 3000:3000 *tag-name*\
> \
> `-d` will run in detached mode\
> `-p` port forwarding option (short version)

## DockerHub
You can share your app using dockerhub. If you are new to DockerHub, it is something similar to GitHub but with docker containers :)

1. Login to DockerHub
> docker `login` -u YOUR-USER-NAME
2. Use a tag to identify your container
> docker `tag` *tag-name* YOUR-USER-NAME/tag-name
3. Push your changes
> docker `push` YOUR-USER-NAME/tag-name

## Play with docker
You can test your container online with https://labs.play-with-docker.com and run the image using the command docker `run`

> docker `run` -dp 3000:3000 *YOUR-USER-NAME/getting-started*

**note**: Make sure you use the DockerHub `YOUR-USER-NAME/tag-name` and not the local `tag-name` as this will pull the image from `DockerHub`

## Volumes
If you want to persist the data, you should use **volumes**. Think of volumes as a simple bucket of data
1. Create a volume
> docker `volume` create *volume-name*
2. Run the container but binding the **volume** to the route you want to persist the data to
> docker `run` -dp 3000:3000 -v *volume-name:/path-to-data* **tag-name**

> `-v` option will use the previously created volume and will use the **path-to-data** to make the data persist
3. Use the volume inspect command if you want to know more about the volume
> docker `volume` inspect *volume-name*

## Dev Mode Container
To run the container in dev mode `hot reloading` to have the changes in your code base reflected inside the container:
1. Run the following command


    docker run -dp 3000:3000
    -w /app -v "$(pwd):/app"
    node:18-alpine
    sh -c "yarn install && yarn run dev"

> `-dp 3000:3000` same as before port forwarding

> `-w /app` working directory *inside* the container

> `-v "$(pwd):/app"` *bind mount* (link) the **host** present dir with the **containers** route dir `/app` but docker requires absolute path, for that we use `psd` instruction

> `node:18-alpine` image to use

> `sh -c` initial command to run

2. if you want to see the logs, use the instruction:
> docker `logs` -f *container-id*
3. Do any kind of changes in the code, and you will see them reflected

## Multi-container Apps (Docker Compose)
Usually an app is build using different services, and to achieve that you can deploy multiple containers each one with one single responsibility but connected by one *internal* `network`

## Other useful commands
> docker `exec` *container-id*  command
> docker `logs` -f *container-id*

# Resources
> https://www.dockerhub.com

> https://labs.play-with-docker.com

> `//Official docs from docker getting started`\
> docker `run` -d -p 80:80 docker/getting-started

