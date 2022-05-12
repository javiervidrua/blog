---
layout: post
title:  "Containerization with Docker"
date:   2021-09-22 00:00:00 +0200
categories: jekyll update
---

## Containerization

Containerization is a process that consists of packaging code and all of its dependencies, so it can be moved from one execution environment to another and executed quickly and reliably.

It is a type of OS virtualization, through which applications run and are isolated in what are called "containers".

### Benefits

There are many benefits to using virtualization:
* Portability
* Scalability
* Faster deployment
* High productivity
* Enhanced security
* Continuity (if one fails, it does not affect the others)

### Differences with virtualization

Virtualization uses software to create an abstraction layer over the computer hardware, so they can be divided and used as if there were multiple computers (called VMs). Each VM has its own OS.

With virtualization, you can run multiple computers under the same real computer, but with containerization, you can run multiple services under the same OS on a single computer.

Furthermore, virtualization is a heavyweight operation, in comparison to containerization, which is a lightweight operation.

On one hand, virtualization provides complete isolation, on the other hand, containerization provides less isolation from the host OS.

But the most important difference is that virtualization is not portable, where as containerization was built to be portable.

## Docker

Docker is the most well-known containerization tool. It goes by the keywords "develop", "ship" and "run" anywhere.

It provides what is called Docker Platform, that allows for container development, testing and deployment.

Docker uses a client-server architecture, so the client and the daemon can be in different servers.

The architecture components are:
* Docker Host: Includes the Docker daemon (one of the most core components), which handles Docker Objects such as images, containers, networks and volumes.
* Docker Client: Transmits the commands to the Docker deamon, which executes it.
* Docker Registries: They store Docker Images that can be downloaded.
* Docker Objects: They are the following:
  * Image: Read-only template.
  * Container: Created when an image is run.
  * Volume: Store the persistent data of the cointainers.
  * Network: How the isolated containers communicate with one another. It has several network drivers.
* Docker Engine: The core part of the Docker system. It is an application that follows a client-server architecture. It has several components:
  * Server: The daemon.
  * REST API: To communicate with the Docker daemon.
  * Docker CLI: Client used to enter Docker commands.

### Installation

For Ubuntu systems, [this](https://docs.docker.com/engine/install/ubuntu/) is the webpage with the installation instructions.

The following is a script that does everything automatically, with no configuration required:
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

To get the configuration of the system, run `docker info`.

### Images

To create a Docker Image, you can write a Dockerfile using a simple syntax that defines the steps required to set everything up.

Here is a list of the basic commands to use and manipulate images:
* `docker image build`: Create an image from a Docker file.
* `docker image history`: Display an image's history.
* `docker image inspect`: Display detailed information about one or more images.
* `docker image ls`: Lists the images.
* `docker image prune`: Removes unused images.
* `docker image pull`: Downloads an image from a registry.
* `docker image push`: Pushes an image to a registry.
* `docker image rm`: Deletes one or more images.
* `docker image save`: Saves an image to a .tar archive.

### Containers

* `docker container ls`: Show the running containers.
* `docker container stop <container_name>`: Stop the container named \<container_name\>.

### Networks

* `docker network ls`: List the networks.

### Network drivers

Docker has several network drivers available:
* Bridge: Interconnects containers running under the same daemon.
* Host: The container shares the hosts network, but it does not have its own IP address.
* Overlay: Connects two or more Docker daemon hosts to form a distributed network. It can be used with encryption enabled so the communications are secure.
* Macvlan: Some legacy apps demand to be connected to the physical network. This driver can be used to assign a MAC address to each container's virtual network interface.
* None: Used to completely disable the network on a container.

### Docker usage examples

Build a custom image from a Dockerfile on the current working directory:
```bash
docker build -t new_image_name .
```

Run a Python3 program that's on the current working directory on a Python3 container and remove the container when the program finishes:
```bash
docker run --rm -v$(pwd):/src python:3 python/src/program.py
```

Start a Python3 container in iteractive mode (open the interpreter shell):
```bash
docker run --rm -it -v$(pwd):/src python:3
```

Start a Python3 container and open a Bash shell:
```bash
docker run --rm -it -v$(pwd):/src python:3 /bin/bash
```

Create a Dockerfile from the base image of Python3 and add numpy to it, then build the image from the Dockerfile of the current working directory (*.*):
```bash
cat > Dockerfile << EOF
FROM python:3

RUN pip3 install numpy
EOF
docker build -t <new_image_name> .
```

Start an Nginx container to serve files of the current working directory, forwarding the port 80 of the host to the port 80 of the container. As Nginx is a web server, it will keep running until we stop it:
```bash
docker run --rm -v$(pwd):/usr/share/nginx/html -p 80:80 nginx:latest
```

The same as the last one, but run the container in background mode (*-d* for *daemon*):
```bash
docker run --rm -v$(pwd):/usr/share/nginx/html -p 80:80 -d nginx:latest
```

Open a shell on a container running in the background:
```bash
docker exec -it <container_name> /bin/bash
```

Create a network, a Mysql container in the background (setting the environment variable MYSQL_ROOT_PASSWORD to 'root') and a Node.js container with an open Bash shell. **Note that Docker automatically sets up DNS entries so the containers can communicate with each other using their names**:
```bash
docker network create <network_name>
docker run --rm -d --name mysql --net <network_name> -e MYSQL_ROOT_PASSWORD='root' mysql:5.6
docker run --rm -it --name nodejs --net <network_name> nodejs:8 /bin/bash
```

## Docker Compose

The best way to describe Docker Compose is "Docker on steroids", is a tool that allows users to define and run multi-container Docker applications.

It uses a .yaml file that has the configuration for your application services. Once that gets defined, you can start everything with one command.

It is compatible with the following environments:
* Production
* Staging
* Development
* Testing
* Continuous Integration

### Steps to use Docker Compose

These are the steps to start using Docker Compose:
1. Create a Dockerfile.
1. Create docker-compose.yml
1. Run `docker-compose up`.

The following is a docker-compose.yml example file:
```yaml
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code
      - logvolume01:/var/log
    links:
      - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
```

### Docker Compose commands

Here is a list of the basic Docker Compose commands:
* `docker-compose build`: Build or rebuild services.
* `docker-componse create`: Create containers for a service.
* `docker-compose down`: Stop and remove containers and networks.
* `docker-compose kill`: Force stop service containers.
* `docker-compose ls`: List running compose projects.
* `docker-compose pause`: Pause the services.
* `docker-compose ps`: List containers.
* `docker-compose restart`: Restart containers.
* `docker-compose rm`: Removed stopped service containers.
* `docker-compose start`: Start services.
* `docker-compose stop`: Stop services.
* `docker-compose up`: Create and start containers.
* `docker-compose up -d`: Create and start containers in the background.
* `docker-compose up -f docker-compose.json`: Create and start containers defined in a JSON file.

### Examples


#### Example 1

You can use a JSON file instead of a YML file, but in that case you have to specify to Docker Compose the file that you want to use.

Start a Redis and a Mysql container. **Note that with Docker Compose, the containers that you start at the same time, they will be connected to the same network automatically**.

docker-compose.yml file:

```yml
version: '3'
services:
  redis:
    container_name: redis
    image: redis:3.2.12
    volumes:
      - ./redis:/data
    restart: always
  mysql:
    container_name: mysql
    image: mysql:5.6
    environment:
      MYSQL_ROOT_PASSWORD: root
    volumes:
      - ./mysql:/var/lib/mysql
    restart: always
```

And then run `docker-compose -d up` to start the containers in the background.

#### Example 2

Start an Nginx server and a Node.js application, and configure the Nginx to act as a proxy for the nodejs application.

lb.conf file (for Nginx):
```conf
upstream serv{
  server nodejs:3000;
}

server {
  listen 8080;
  location / {
    proxy_pass http://serv;
  }
}
```

docker-compose.yml file:
```yml
version: '3'
services:
  nodejs:
    container_name: nodejs
    image: node:latest
    volumes:
      - ./app:/opt/app
    # Run as soon as the container starts
    command: node /opt/app/app.js
  nginx:
    container_name: nginx
    image: nginx:latest
    # Port forwarding
    port:
      - "1313:8080"
    volumes:
      - ./nginx/lb.conf:/etc/nginx/conf.d/lb.conf
    # Wait until the nodejs service has started, then start the nginx service
    depends_on:
      - nodejs
```

And then run `docker-compose -d up` to start the containers in the background.

#### Example 3

The same as the last one, but now use a Dockerfile to build a custom image for the nodejs application, and we start a bunch of nodejs applications and set a load balancer with Nginx.

lb.conf file (for Nginx):
```conf
upstream serv{
  server ex3_nodejs_1:3000;
  server ex3_nodejs_2:3000;
  server ex3_nodejs_3:3000;
  server ex3_nodejs_4:3000;
}

server {
  listen 8080;
  location / {
    proxy_pass http://serv;
  }
}
```

Dockerfile:
```yaml
FROM node:latest
```

docker-compose.yml file:
```yml
version: '3'
services:
  nodejs:
    #container_name: nodejs
    # Use the Dockerfile in the current working directory (.) to build the container image
    build: .
    volumes:
      - ./app:/opt/app
    # Run as soon as the container starts
    command: node /opt/app/app.js
  nginx:
    container_name: nginx
    image: nginx:latest
    # Port forwarding
    port:
      - "1313:8080"
    volumes:
      - ./nginx/lb.conf:/etc/nginx/conf.d/lb.conf
    # Wait until the nodejs service has started, then start the nginx service
    depends_on:
      - nodejs
```

And then run `docker-compose -d up --scale nodejs=4` to start the containers in the background, and start 4 containers of the nodejs application.

## Docker Swarm Mode

Swarm Mode is a cluster management and orchestration tool that's embedded right in the Docker Engine. If you know what Kubernetes is, the best way to describe Swarm Mode is to think about it as a Kubernetes alternative that's easier to use and that I would recommend for simple applications that are not too complex to manage.

Comparing it to Docker Compose, and its ability to define and run containers, you can define and run Swarm service stacks.

If you want detailed information, check the [official page](https://docs.docker.com/engine/swarm/).

### Key concepts

#### Node

A node is an **instance of a Docker Engine that takes part in the swarm**. You can have one or more on a single computer, but usually they are distributed across multiple machines. There are two types:
* Manager node: Dispatches the work (tasks) to the worker nodes. By default, they act as worker nodes too.
* Worker node: Receives and execute tasks dispatched from the manager nodes. It notifies the manager of the current state of operation periodically.

#### Service

It is the **definition of the tasks that will get executed**.

A service specification dictates the container image to use and the commands to execute inside the running containers.

In the replicated services model, the swarm manager sends replica tasks to the worker nodes, and for global services, the swarm runs one task for the service on every node of the cluster.

#### Task

It is made of a container and the commands to run inside said container. It is the **minimum unit of swarm**. Once a task gets assigned to a node, it can only run on the assigned node.

#### Load balancing

The manager node uses **ingress load balancing**, so the services you want are made available to the Internet. If a port is not specified, is uses a port from the range 30000-32767.

Like Docker Compose, Swarm Mode automatically assigns DNS records to each service, and uses internal load balancing to distribute requests between the services based on the DNS names.

### Getting started

In order to be able to use Swarm Mode there are several prerequisites that need to be satisfied:
* Three Linux hosts that can communicate over a network, with Docker installed.
* The IP address of the manager. You should use a fixed IP address for the manager as all nodes in the swarm need to connect to it.
* The ports 2377, 7946 and 4789 (and 50 if you plan to use `--opt encrypted` to use secure connections between the machines) open and allowed by default.

#### Creating a swarm

Once those requirements are met, you can create a swarm. To do so, connect to the manager node and run the following command:
```bash
docker swarm init --advertise-addr <manager_IP>
```

That will output something like:
```
Swarm initialized: current node (xi3zvyg3wgnv04jz0z36b9kyn) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-0yjkvziblj2nuwz35cl9uc7evx3p93onhbxan66gz7wsvwi7tp-02kyqwtgpjotdbzqh0bu1nj2g 10.10.10.10:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

If you clear the terminal, you can get the join command for a worker by running `docker swarm join-token worker`.

Now we can run the following command to check the status of the nodes:
```bash
docker node ls
```

It will output something like (note the *, it means that Docker is connected to it):

```
ID                            HOSTNAME   STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
xi3zvyg3wgnv04jz0z36b9kyn *   ubuntu     Ready     Active         Leader           20.10.8
```

#### Adding worker nodes to the swarm

You can run the join command provided when the swarm was initialized on the worker nodes to join the swarm. In my case:
```bash
docker swarm join --token SWMTKN-1-0yjkvziblj2nuwz35cl9uc7evx3p93onhbxan66gz7wsvwi7tp-02kyqwtgpjotdbzqh0bu1nj2g 10.10.10.10:2377
```

And it will output:
```
This node joined a swarm as a worker.
```

Now, on the manager node, we can run `docker node ls`, and we should see the new node there:
```
ID                            HOSTNAME       STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
xi3zvyg3wgnv04jz0z36b9kyn *   ubuntu         Ready     Active         Leader           20.10.8
yox9u6ux9aom7w5mtal5woqog     ubuntuserver   Ready     Active                          20.10.8
```

#### Deploying a service to the swarm

From the manager node, create a service:
```bash
docker service create --replicas 1 --name test alpine ping docker.com
```

That will create a service named "test", with 1 replica, using the image python tagged 3 that runs the command `ping docker.com`.

Now you can inspect the services running with the command `docker service ls`.

#### Inspecting a service

Run the following command to get the output in JSON format:
```bash
docker service inspect <ID>
```

Add `--pretty` to the above command to get a more readable output.

To see which nodes are running the service, run `docker service ps <service_name | service_ID>`. Either way, it will output something like:
```
ID             NAME      IMAGE           NODE      DESIRED STATE   CURRENT STATE           ERROR     PORTS
7ihrx5jn31fr   test.1    alpine:latest   ubuntu    Running         Running 5 minutes ago
```

#### Scaling the service

Scaling a service is really easy with Swarm Mode, just run `docker service scale <service_name | service_ID>=<number_of_replicas>`.

It will output:
```
test scaled to 3
overall progress: 3 out of 3 tasks 
1/3: running   [==================================================>] 
2/3: running   [==================================================>] 
3/3: running   [==================================================>] 
verify: Service converged
```

Now if we list the containers of the service with `docker service ps <service_name | service_ID>`, we get:
```
ID             NAME      IMAGE           NODE           DESIRED STATE   CURRENT STATE            ERROR     PORTS
7ihrx5jn31fr   test.1    alpine:latest   ubuntu         Running         Running 11 minutes ago             
z6ctn65c04h6   test.2    alpine:latest   ubuntuserver   Running         Running 32 seconds ago             
ahmdt70a0je5   test.3    alpine:latest   ubuntuserver   Running         Running 32 seconds ago
```

#### Deleting a service

Run the command `docker service rm <service_name | service_ID>`.

It will take a few seconds (30s) for the containers to be cleaned up. You can check the state with the command `docker container ls` or with `docker ps`.

#### More detailed info

Check these pages:
* [https://docs.docker.com/engine/swarm/networking/#configure-service-discovery](https://docs.docker.com/engine/swarm/networking/#configure-service-discovery)
* [https://docs.docker.com/engine/swarm/ingress/](https://docs.docker.com/engine/swarm/ingress/)