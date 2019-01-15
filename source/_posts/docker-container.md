---
title: docker-container commands and its architecture
tags: [docker]
categories: [docker]
date: 2019-01-07 13:51:59
---

# Docker Container

The instances of Docker Images are containers which could be runned via `run` command and reveals the basic purpose of Docker.

## Running a container and listing containers

Using `it` to run docker containers in an interactive mode.

```bash
$ sudo docker run –it {image name or ID} /bin/bash 
```

> Because `exit` will just kill the borned container.,you can use `Ctrl+P+Q` to exit the interactive shell with the contianer reserved. 

> `nsenter` command could achieve the very same thing.——[nsenter](https://hub.docker.com/r/jpetazzo/nsenter/dockerfile/)

Using `ps` command to list docker containers

```bash
$ sudo docker ps
$ sudo docker ps -a	# returns all of the containers on the system.
```

<!-- more -->

## Command history of an image

> The above command will show all the commands that were run against the **74b2f51293f9** image.

```bash
$ sudo docker images 
REPOSITORY                      TAG                 IMAGE ID            CREATED             SIZE
envoyproxy/envoy-build-ubuntu   my_tag              74b2f51293f9        3 days ago          3.12GB
hello-world                     latest              fce289e99eb9        6 days ago          1.84kB
ubuntu                          xenial              b0ef3016420a        9 days ago          117MB

$ sudo docker history 74b2f51293f9
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
74b2f51293f9        3 days ago          /bin/sh -c ./build_container_ubuntu.sh          3GB                 
7e9dc826f07e        3 days ago          /bin/sh -c #(nop) COPY file:9326d4a60f409f2a…   1.16kB              
aa8519dc1d46        3 days ago          /bin/sh -c #(nop) COPY multi:bb2b042aef38ecd…   10kB                
2909d73adcf9        3 days ago          /bin/sh -c #(nop) COPY dir:ad387371ef9352f92…   119kB               
f6d5365aeac1        3 days ago          /bin/sh -c #(nop) COPY dir:b71e9522a30046ee6…   1.06MB              
e7568c3b205d        3 days ago          /bin/sh -c #(nop) COPY file:60d95c86360ded7b…   432B                
c80abb178eef        3 days ago          /bin/sh -c #(nop) COPY multi:681cbdd7f492ada…   5.15kB              
b0ef3016420a        9 days ago          /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B                  
<missing>           9 days ago          /bin/sh -c mkdir -p /run/systemd && echo 'do…   7B                  
<missing>           9 days ago          /bin/sh -c rm -rf /var/lib/apt/lists/*          0B                  
<missing>           9 days ago          /bin/sh -c set -xe   && echo '#!/bin/sh' > /…   745B                
<missing>           9 days ago          /bin/sh -c #(nop) ADD file:7f95be7c8278786d5…   117MB 
```

## Docker Top

```bash
$ sudo docker ps	# list running containers.
$ sudo docker top {ContainerID}	# this command is similar to linux top.
```

## Docker stop to stop a running container

```bash
$ sudo docker stop {containerID}
```

## Docker rm to delete a container

```bash
$ sudo docker rm {containerID}
```

## Docker stats to show statistics of a running container

```bash
$ sudo docker stats {containerID}
```

## Docker attach to attach to a running container

```bash
$ sudo docker attach ContainerID
```

> Once you have attached to the Docker container, you can run the above command to see the process utilization in that Docker container. [docker guide](Once you have attached to the Docker container, you can run the above command to see the process utilization in that Docker container.)

## docker pause/unpause/kill to pause/unpause/kill the preccesses in a running container

```bash
$ sudo docker pause/unpause/kill {containerID}
```

## Docker-Container Lifecycle

![](/images/docker/container-lifecycle.jpg)

- Initially, the Docker container will be in the created state.
- Then the Docker container goes into the running state when the Docker run command is used.
- The Docker kill command is used to kill an existing Docker container.
- The Docker pause command is used to pause an existing Docker container.
- The Docker stop command is used to pause an existing Docker container.
- The Docker run command is used to put a container back from a stopped state to a running state.

## Docker Architecture

### Tarditional and standard architecture of virtualization

![](/images/docker/architecture.jpg)

- The server is the physical server that is used to host multiple virtual machines.
- The Host OS is the base machine such as Linux or Windows.
- The Hypervisor is either VMWare or Windows Hyper V that is used to host virtual machines.
- You would then install multiple operating systems as virtual machines on top of the existing hypervisor as Guest OS.
- You would then host your applications on top of each Guest OS.

### New generation of virtualization that is enabled via Dockers

![](/images/docker/architecture-docker.jpg)

- The server is the physical server that is used to host multiple virtual machines. So this layer remains the same.
- The Host OS is the base machine such as Linux or Windows. So this layer remains the same.
- Now comes the new generation which is the Docker engine. This is used to run the operating system which earlier used to be virtual machines as Docker containers.
- All of the Apps now run as Docker containers.

---------------------

# References

[Docker Guide](https://www.tutorialspoint.com/docker/docker_quick_guide.htm)
