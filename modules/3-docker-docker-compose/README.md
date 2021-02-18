# Docker and Docker-compose: Application Containerization:

## 1. Docker Container Engine:

Docker is a container engine that enables OS-level virtualization. It's widely adopted by communities and enterprises for packaging software components for development and deployment.


Check out the Docker overview from the official docs page: https://docs.docker.com/get-started/overview

### a. Docker Concepts:

- **Container Engine**: the main OS-level virtualization component, which replaces the hypervisor in classic virtualization and removes the need for installing a full operating system by sharing the kernel with the host
- **Container Image**: a collection of read-only Linux filesystem layers built on top of a base filesystem layer (e.g. Debian Linux distribution), assembled based on the instructions from a `Dockerfile`, and identified by a name/tag combination or a unique ID
- **Container**: a container image (static filesystem layers) with a writeable filesystem layer, live, running on a container engine
- **Volume**: a file or directory inside a running container that's persisted in the host's storage (in a known location configured by Docker) after the container is stopped, enabling the container to retain its data across restarts
- **Network**: means to access network-based services provided by applications inside containers from the host, the other way around, or to enable inter-container communication, based on a bridge architecture by default
- **Registry**: a Content Delivery System for distributing Docker images, available with local installations of Docker (for working locally) and in remote servers (public ones like Docker-Hub or private ones like Google Container Registry or Gitlab Registry)


***Additional points to explore:***
- Docker computes a SHA256 hash for each layer of an image it's building, computed mainly using the contents of the layer (file contents and hierarchy). With that, layers are not duplicated in storage, so multiple images that have a shared base will be using the same layers.  
- Docker image layers are usually formed not just by adding files and directories from the host's storage or external networks, but also by executing commands
- Docker containers can be attached to terminals to enable interacting with it through a shell
- Docker supports fixing limited CPU, RAM, and GPU resources for running containers
- Docker enables using other types of networks besides the default `bridge`, which are `host`, `overlay`, and `macvlan`
- Docker interacts with the operating system using a daemon binary called `dockerd`
- Dockerfiles can be split into multiple stages, enabling the build of multiple images on top of each other or on top of other bases, enabling the use of a single Dockerfile to create different images targeting specific environments or platforms


### b. Docker Commands:

- `docker images`: lists available docker images
- `docker build`: builds an image from a Dockerfile and a context (the location where the `build` command is triggered, and from where the `Dockerfile` instructions will take files/directories if applicable)
- `docker tag`: changes the name/tag of an image to another name/tag combo, often used to rename local images into a name for a remote registry 
- `docker push`: upload an image to a remote registry
- `docker pull`: download an image from a remote registry
- `docker run`: creates and starts a container from an image, optionally attaching a terminal into it
- `docker exec`: attach a terminal into a running container
- `docker rm`: removes an image from the local registry
- `docker rmi`: removes an image from the local registry
- ``: list, create, or remove volumes
- `docker system df`: review disk usage 
- `docker system prune`: clear images, volumes, networks, ..., optionally only

***Additional commands to explore:***
- `docker create`
- `docker start`
- `docker stop`
- `docker logs`
- `docker kill`
- `docker network`



## 2. Docker-Compose Container Definition and Management:

Docker-Compose is a tool for easily defining and running multi-container systems. It replaces the use of ad-hoc commands to build and run different containers using a structured YAML file. The structure of this configuration file depends on the version of Docker-Compose in use and is specified as an attribute (e.g. `version: "3.7"`).

Docker-Compose is installed independently from the Docker container engine.

For further info, read the official Docker docs' Docker-compose overview: https://docs.docker.com/compose

### a. Docker-Compose Concepts:

Docker-Compose doesn't add a lot of new concepts to the Docker ecosystem. It uses the term **Service** for each container in the configuration, and each service is:
- Based on an existing image or built from a Dockerfile, a given context, and a list of build arguments if applicable
- Has a list of volumes and bind mounts
- Has a list of exposed ports and port forwards
- Has a list of environment variables
- Depends on one or more other services, so it will not be started unless the other services are up and running

Among other attributes related to managing the container.


### b. Docker-Compose Commands:

- `docker-compose build`: build the images required for services if applicable
- `docker-compose up`: creates and starts service containers
- `docker-compose ps`: inspects the running service containers 
- `docker-compose down`: stops the running service containers
- `docker-compose rm`: removes the stopped service containers