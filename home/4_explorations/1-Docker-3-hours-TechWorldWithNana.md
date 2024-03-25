# Docker (3 hours course)

Created time: March 25, 2024 3:08 PM
Description: Tech world with Nana
Files & media: https://youtu.be/3c-iBn73dDE?si=KRVVmXYVYpRCaYfY
PKB: containers (https://www.notion.so/containers-9d9ba8b0f6f34c7bb4a414567cec7a02?pvs=21)
Progress: Watched completely, practical remains
Project: Backend dev (https://www.notion.so/Backend-dev-c813583e149540c092e24583384a554a?pvs=21)
Status: Practical remains

Links

- [Docker on Termux](https://gist.github.com/oofnikj/e79aef095cd08756f7f26ed244355d62)
- https://github.com/jackyzha0/docker-explained
- [Link to same Notion note](https://www.notion.so/sanjarcode/Docker-3-hours-course-7134fece893f4211a47f14fdc3081544)

## [01:58](https://www.youtube.com/watch?v=3c-iBn73dDE&t=118s) - What is Docker? (TBC)

[What is Docker](https://github.com/jackyzha0/docker-explained?tab=readme-ov-file#what-is-docker) - Docker is a tool that makes it really easy to package applications into self-sustaining 'containers'.

[Why Docker](https://github.com/jackyzha0/docker-explained?tab=readme-ov-file#why-docker) - Docker makes it super easy to work with these containers and, by proxy, you can get all the cool benefits of containers easily too! It also allows you to programmatically define a container through code, meaning you can collaborate and work on Docker containers just as you would with a regular piece of code through version control like `git`.

## [10:56](https://www.youtube.com/watch?v=3c-iBn73dDE&t=656s) - What is a Container? (TBC)

- [What are containers?](https://github.com/jackyzha0/docker-explained?tab=readme-ov-file#what-are-containers)

    Containers, as their name suggests, contain things. In the case of Docker, these contain *all* the parts the application needs to run, everything from libraries and dependencies to your actual app source code. This means that 'containerized' applications don't need to rely on a system to have certain dependencies (e.g. `Node.js`) installed on the user's system to run because the container will have it packaged. *You can think of Docker containers like a fully self-contained and running version of your application.*

- A “container” is a running instance of an “image”. Full map: Dockerfile > image > container > compute, network and file system of the container
- It has its own ports and file system (this is not persistent by default and is cleared if container is shut down). For persistent files, like for databases, one has to use “volumes”.
- [Why containers](https://github.com/jackyzha0/docker-explained?tab=readme-ov-file#why-containers)

    Containerization means that everything to do with your application stays inside the container. You shouldn't need to worry about how stuff on your machine (e.g. which version of Python you have) affects how your program runs. As a side benefit, this means that Docker containers are dependency-free. Never worry about "oh, it works on my machine" ever again! After a Docker image is created, all of its contents are frozen so it should work exactly the same on your computer as it does for someone else (assuming you both have Docker).


## [19:40](https://www.youtube.com/watch?v=3c-iBn73dDE&t=1180s) - Docker vs Virtual Machine

- Docker
    - Lightweight
    - Uses hosts kernel
    - Needs a basic level of hw and os to run natively on non Linux platforms like Win, Mac. But as of now, all platforms can run natively run Docker, so we’re good.
- VM
    - Takes up more space and resources.
    - Has own kernel different from host.
    - Can run on any machine.


## [23:53](https://www.youtube.com/watch?v=3c-iBn73dDE&t=1433s) - Docker Installation

## [42:02](https://www.youtube.com/watch?v=3c-iBn73dDE&t=2522s) - Main Docker Commands (TBC, TBT)

## [57:15](https://www.youtube.com/watch?v=3c-iBn73dDE&t=3435s) - Debugging a Container (TBT)

To enter the terminal of a container, use

`docker exec -it container_id /bin/bash` (or bin/sh for worst case containers), this starts a terminal session that’s running in the container.

Some commands like `curl` may not be available in some very specific containers. This means containers have a limited terminal and functionality unless it already exists or installed (WHICH?)

## [1:06:39](https://www.youtube.com/watch?v=3c-iBn73dDE&t=3999s) - Demo Project Overview - Docker in Practice (TBC, TBT)

- Images why the extra word than Dockerfile?

    What the fuck is an image? I always get confused by the difference of a Dockerfile with an image, why do we need two terms? (still don’t know).

    What I can guess now is that Dockerfile is something you write by hand and is human readable. While an image is “an entry that appears when you do `docker image ls` (assuming you built an image with the Dockerfile using `docker build`). It’s a binary thing that is in the runtime, ready to run. Ah, it makes sense that during build, you pull base images and dependency images, so the image listen actually contains the dependencies code really in substance.

- [Layers](https://github.com/jackyzha0/docker-explained?tab=readme-ov-file#layers)

    Docker images, like ogres (or cakes if you're a boring person), have many layers. The base layer often provides some basic functionality like providing `git`, `bash` or `apt` -- otherwise your container has nothing to run off of! We can then add our own layers on top of that base layer, like installing dependencies, copying files into the image, and defining the command to run when the container starts up. These instructions are programmatically defined through a Dockerfile.

- Networking host-container and container-container

    Docker as a runtime maintains a network, and of course all containers have their port too.

    - Network binding - any image can be setup to bind to a host port with a port of the container, the command is [x…z](https://docs.docker.com/desktop/networking/#networking-features). To access the service on the host, we use localhost:hostSidePort as setup in the binding. The container side port is irrelevant to the host.
    - Inter container communication - containers can communicate with each other too. To access containerB in containerA, we don’t use `[localhost](http://localhost)` keyword or even the port, merely the container/service name (as visible in `docker ps`) is enough, e.g.
        - ‘cranky_engelbart/home_page` instead of localhost:8080
        - ‘mongodb://admin:password@myMongoContainer’ instead of ‘mongodb://admin:password@localhost:27017’

## [1:29:49](https://www.youtube.com/watch?v=3c-iBn73dDE&t=5389s) - Docker Compose - Running multiple services

We needed two containers in our experiment and started them manually using the CLI. This can get tedious when there are more containers or hw machined involved.

`docker-compose` is a command (and tool) in the default docker suite (already installed) that helps us automate this.

The command takes a YAML file called a “Docker compose file” that lists down the container (names), images and other inputs (just like we passed to the `docker` command). **

*This file is usually stored in the codebase itself.*

The command starts all the containers listed in the file for us with a single command.

Additionally:

1. **Creates network automatically** - Docker compose creates a network automatically for us, and tears down the network when the containers are shutdown (using `docker-compose -f file_name.yaml down`)
2. **Inter-container dependency automatically handled** - When one containers depends on other, like a server app container pinging the database container, docker-compose keeps them on until the dependency has fully started up. This does not need to be specified in the file, it happens automatically.
3. Adds names to containers and the network automatically

And like usual, the containers and network is visible vis `docker ps`

## [1:42:02](https://www.youtube.com/watch?v=3c-iBn73dDE&t=6122s) - Dockerfile - Building our own Docker Image

The `Dockerfile` is a file used to generate an image. It is not needed generally, and but is used when we want to have custom images, and therefore custom containers. The file is should exactly be named `Dockerfile`

**Base image - e**very dockerfile starts with a base image, something like ubuntu, Node etc. These base images exist already on Dockerhub.

Its preferable to select a higher level base image like node over ubuntu, because then we won’t have to install node on it, and as discussed previously, node will have some sort of underlying layers already.

Docker file usually has statements with a keyword and then the argument. For example:

```jsx
FROM node // base image, for specific version use `node:13-alpine`

ENV MONGO_DB_USERNAME=admin \
		MONGO_DB_PWD=password

RUN mkdir -p /home/app // note: this
COPY . /home/app
CMD ["node", "./home/app/server.js"]
```

- `RUN` command is used to execute any Linux command.
- `COPY` compared to `RUN cp`. It runs on the host file system, as opposed to running as a command in the container. This is essential for easily copying stuff from host to containers.
- `CMD`  also called the “entry point command”. Uses to mark the begin of operation of the container when it is instantiated. This differs from `RUN` in that a dockerfile can have many `RUN` but only a single `CMD` command.

Usually, node_modules and other dependencies are generally copied into (into the to be container) in the `Dockerfile` since the USP of Docker is to avoid any kind of processing needed for dependencies that one could get stuck on.

*The Dockerfile is also, like compose, usually stored in the codebase itself. But it’s not needed to be present in the container, nor is the compose file, btw.*

### Using Dockerfile

`docker build -t my-app:1.0 .` (if same folder as `Dockerfile`), here the 1.0 is called a “tag”. Tags can be words too, but have to follow the colon syntax.

After running above command, the image is created and can be seen using `docker images`.

Now one can spin up containers from this image using `docker run`

## Updating a Dockerfile

When a Dockerfile is edited, one must shut down and delete existing images, and `docker build` the new image (WHY?)

To delete an image, use `docker rm image_id`, to see image ids, use `docker images`

## [2:04:36](https://www.youtube.com/watch?v=3c-iBn73dDE&t=7476s) - Private Docker Repository - Pushing our built Docker Image into a private Registry on AWS (TBC, TBT)

prereqs for AWS: Set up AWS ECR in the cloud console, then setup aws cli and credentials.

`docker` itself can push images to destination quite easily. Steps:

1. Log in to the registry (storage destination), using `docker login URL`
2. Create an image named properly with registry prefix. Suppose you have `my-app:1.0` as the image you created from own Dockerfile, to push, clone and name it properly in one go using command `docker tag my-app:1.0 xyz.abc.com/my-app:1.0` . If you check images, a new image can be seen.
3. Since the name takes care of the destination, we just need to push. `docker push xyz.abc.com/my-app:1.0`

That’s it. See result:

```bash
[~]$ docker push 664574038682.dkr.ecr.eu-central-1.amazonaws.com/my-app:1.0
The push refers to a repository [664574038682.dkr.ecr.eu-central-1.amazonaws.com/my-app]
5678c8aa26b3: Pushed
bb2a17dfd6c2: Pushed
099773e542db: Pushed
9efd3ca0eab1: Pushed
a721b64d51de: Pushed
77cae8ab23bf: Pushed
1.0: digest: sha256:fc8aeac852dcb040b727cfb222078a27305c05bb480da50ecaß86ca7ce398bf9 size: 1576
```

- Docker image name convention

    Actually, the proper template for a image name is `regsitryDomain/imageName:tag` , but for Dockerhub the whole thing is not needed, and only imageName or imageName + tag is enough for pull and push. But for custom registeries the full name is needed. This can be done by clone and copy functionality using the `docker tag existingImage newImage` command like we did for AWS.

- Editing the image, how to update?

    When you change app code. Just recreate the image using the Dockerfile, then tag the newly created image with a bumped up tag, and push. You’re done. AWS ECR allows multiple images (with differing tags of course) to be pushed, upto 1000 images max.

- How to use/pull own image from own/custom registry

    Suppose you have logged into a development server.

    Make sure you are authenticated with the registry (which we have done already using `docker login registry-url-xyz`)

    - If it’s a single image, just `docker pull` and `docker run`, simple.
    - If a docker-compose file, just add a service with the image with value as complete image name of your image. Other containers can stay in the file like usual. To start the server, just start all the containers using `docker-compose -f compose-file.yaml`


## [2:19:06](https://www.youtube.com/watch?v=3c-iBn73dDE&t=8346s) - Deploy our containerized app (TBC)

Steps:

1. Install docker.
2. `docker login myRegistryURL`
3. `docker pull myImageCompleteName`
4. Then run the image, or the compose file. Done.

## [2:27:26](https://www.youtube.com/watch?v=3c-iBn73dDE&t=8846s) - Docker Volumes - Persist data in Docker (TBT)

By default a container’s file system (actually a virtual file system) has no persistence, i.e. when the container is turned off, the data is also gone. This is solved by Docker “volumes”.

The basic idea is that a location of the host file system (physical file system( is mounted inside a container’s file system (virtual file system), this syncs changes both ways.

There are 3 types of volumes:

1. Host volume, specify both - `docker run -v host_location:container_location`
2. Anonymous volumes - specify just container location - `docker run -v container_location`, this creates and mounts a default host location that Docker specifies automatically
3. Named volumes specify host folder name and container location - `docker run -v name:container_location` . One advantage is that multiple containers can use the same volume too. Also, named volumes are the most widely used and good approach. See [code-in-docker-compose-file](https://youtu.be/3c-iBn73dDE?si=iHfYsD30lrAaTa0F&t=9141)

## [2:33:03](https://www.youtube.com/watch?v=3c-iBn73dDE&t=9183s) - Volumes Demo - Configure persistence for our demo project

Where do named volumes reside on the host?

- Windows - `C:\ProgramData\docker\volumes`
- Linux - `/var/lib/docker/volumes`
- Mac - same as linux

Inside this directory, there are directories with volumes names or hash (for anonymous volumes) are present.

## [2:45:13](https://www.youtube.com/watch?v=3c-iBn73dDE&t=9913s) - Wrap Up

Learnt important stuff. Now, to manage many nodes and install stuff dynamically, docker-compose isn’t enough, since it’s not dynamic, it just strings together and starts stuff (akaik).

Kubernetes, aka k8s solves this problem of multi node dynamic container orchestraion.
