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

## Contents
### Building the image
Taking a look at the `Dockerfile` that is the command used to build the image and then run:
> `// tag-name is the name of the app `\
> `// "." as parameter will tell docker to use Dockerfile in the root of the folder`\
> docker `build` -t *tag-name* .

![build](/docs/build.png "build")

### Starting a Container
Now that we have an image, we should start the container
> docker `run` -dp 3000:3000 getting-started

After a few seconds you will see the webapp running in
> http://localhost:3000

### Implementing changes
You can do any change in the app, and then you can deploy again
**make sure you stop and remove the container first!**

> `// list all running containers to get container-id`\
> docker `ps`

> docker `stop` *container-id*

> docker `rm` *container-id*



## Resources
> https://www.dockerhub.com

> https://labs.play-with-docker.com

> `//Official docs from docker getting started`\
> docker `run` -d -p 80:80 docker/getting-started

