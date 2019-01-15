---
title: docker-image
tags: [docker]
categories: [docker]
date: 2019-01-07 13:07:30
---

# How does docker image works?

> *In docker, everything is based on images. An image is a combination of a file system and parameters.*

## Displaying images

```bash
$ sudo docker images
REPOSITORY                      TAG                                        IMAGE ID            CREATED             SIZE
envoyproxy/envoy-build-ubuntu   my_tag                                     74b2f51293f9        3 days ago          3.12GB
hello-world                     latest                                     fce289e99eb9        6 days ago          1.84kB
ubuntu                          xenial                                     b0ef3016420a        9 days ago          117MB
envoyproxy/envoy-build-ubuntu   a734887ad06609cf0b3c023d38239bf3e79d3717   4a7ef69d7f8f        2 weeks ago         3.15GB
```

<!-- more -->

## Downloading Docker Images

We can download docker images from [Docker Hub](https://hub.docker.com/) using `run` command.

```bash
$ sudo docker run {image name}
```

### Example: Run hello-world image

```bash
$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
1b930d010525: Pull complete 
Digest: sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

## Removing Docker Images

Docker images on the system could be removed via `docker rmi {imageID}` command.

```bash
$ sudo docker images 
REPOSITORY                      TAG                                        IMAGE ID            CREATED             SIZE
envoyproxy/envoy-build-ubuntu   my_tag                                     74b2f51293f9        3 days ago          3.12GB
hello-world                     latest                                     fce289e99eb9        6 days ago          1.84kB
ubuntu                          xenial                                     b0ef3016420a        9 days ago          117MB
envoyproxy/envoy-build-ubuntu   a734887ad06609cf0b3c023d38239bf3e79d3717   4a7ef69d7f8f        2 weeks ago         3.15GB

$ sudo docker rmi  4a7ef69d7f8f
Untagged: envoyproxy/envoy-build-ubuntu:a734887ad06609cf0b3c023d38239bf3e79d3717
Untagged: envoyproxy/envoy-build-ubuntu@sha256:465846b962607f6f34fe9779fb6242ae2ad6a8ff663560f03f183874caf30ed9
Deleted: sha256:4a7ef69d7f8f671e7d840d623491e600d880823c39a75c9be87b0d48b5f66050
Deleted: sha256:234198ed09d404674730682092ae7b8eabab161ceb6c560082daf8be2e5adf85
Deleted: sha256:bd3c7082ba45c753678365fc1229302f5328695d4e0f50da4350d5b67c020674
Deleted: sha256:47a4a337611e093e075cf901f18f1fbdd9dc77c1a485eb6bfd0183f77f2704bb
Deleted: sha256:33a7150ec82f04e8cad31a46adb73131a85e53da2703735329d967832d645917
Deleted: sha256:35d10fb3fe7ad6da27d8b37ead79f771ee323a3c0884aed68306b918b8347603
Deleted: sha256:879cb00b82909d2284b63c33a1097bd63148f62b6d75e99d6a134ace6a48ea51
Deleted: sha256:271d6d66fa415b97355386261b367c9f6e8e0526bc3c49aaf9abc11e5ef1a73a
Deleted: sha256:ad38d0b8ff5e07d6875cb39931e2c79fb90cf142584ea813437b013d3639678f
Deleted: sha256:8ae3e0d35735ff77e9ef2a15816747b01316225829ece78dbc41bc50eddb7dfe
Deleted: sha256:a6a57518ff0cc0e30c0e5c964abc052038413f57cd570bd89ab4e4493741a5b3
Deleted: sha256:41c002c8a6fd36397892dc6dc36813aaa1be3298be4de93e4fe1f40b9c358d99

$ sudo docker images 
REPOSITORY                      TAG                 IMAGE ID            CREATED             SIZE
envoyproxy/envoy-build-ubuntu   my_tag              74b2f51293f9        3 days ago          3.12GB
hello-world                     latest              fce289e99eb9        6 days ago          1.84kB
ubuntu                          xenial              b0ef3016420a        9 days ago          117MB
```

## Docker Inspect

`docker inspect {repo name or image ID}` command returns details of the peculiar image.

```bash
$ sudo docker inspect fce289e99eb9
[
    {
        "Id": "sha256:fce289e99eb9bca977dae136fbe2a82b6b7d4c372474c9235adc1741675f587e",
        "RepoTags": [
            "hello-world:latest"
        ],
        "RepoDigests": [
            "hello-world@sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2019-01-01T01:29:27.650294696Z",
        "Container": "8e2caa5a514bb6d8b4f2a2553e9067498d261a0fd83a96aeaaf303943dff6ff9",
        "ContainerConfig": {
            "Hostname": "8e2caa5a514b",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"/hello\"]"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:a6d1aaad8ca65655449a26146699fe9d61240071f6992975be7e720f1cd42440",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {}
        },
        "DockerVersion": "18.06.1-ce",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/hello"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:a6d1aaad8ca65655449a26146699fe9d61240071f6992975be7e720f1cd42440",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 1840,
        "VirtualSize": 1840,
        "GraphDriver": {
            "Data": {
                "MergedDir": "/var/lib/docker/overlay2/ae20a9dd5baaaad804a999a98bf5951bdc2a41a555f79f61d823a97ed8bd87d0/merged",
                "UpperDir": "/var/lib/docker/overlay2/ae20a9dd5baaaad804a999a98bf5951bdc2a41a555f79f61d823a97ed8bd87d0/diff",
                "WorkDir": "/var/lib/docker/overlay2/ae20a9dd5baaaad804a999a98bf5951bdc2a41a555f79f61d823a97ed8bd87d0/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:af0b15c8625bb1938f1d7b17081031f649fd14e6b233688eea3c5483994a66a3"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
```

```bash
$ sudo docker inspect hello-world
[
    {
        "Id": "sha256:fce289e99eb9bca977dae136fbe2a82b6b7d4c372474c9235adc1741675f587e",
        "RepoTags": [
            "hello-world:latest"
        ],
        "RepoDigests": [
            "hello-world@sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2019-01-01T01:29:27.650294696Z",
        "Container": "8e2caa5a514bb6d8b4f2a2553e9067498d261a0fd83a96aeaaf303943dff6ff9",
        "ContainerConfig": {
            "Hostname": "8e2caa5a514b",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"/hello\"]"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:a6d1aaad8ca65655449a26146699fe9d61240071f6992975be7e720f1cd42440",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {}
        },
        "DockerVersion": "18.06.1-ce",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/hello"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:a6d1aaad8ca65655449a26146699fe9d61240071f6992975be7e720f1cd42440",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 1840,
        "VirtualSize": 1840,
        "GraphDriver": {
            "Data": {
                "MergedDir": "/var/lib/docker/overlay2/ae20a9dd5baaaad804a999a98bf5951bdc2a41a555f79f61d823a97ed8bd87d0/merged",
                "UpperDir": "/var/lib/docker/overlay2/ae20a9dd5baaaad804a999a98bf5951bdc2a41a555f79f61d823a97ed8bd87d0/diff",
                "WorkDir": "/var/lib/docker/overlay2/ae20a9dd5baaaad804a999a98bf5951bdc2a41a555f79f61d823a97ed8bd87d0/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:af0b15c8625bb1938f1d7b17081031f649fd14e6b233688eea3c5483994a66a3"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
```

---------------------

## Docker Tag

### Syntax

```bash
docker tag imageID Repositoryname	# tag an image to the relevant repository
```
- `imageID` could be searched via `sudo docker images`
- `Repositoryname` is the repository to which the ImageID needs to be tagged to.

### Example

```bash
$ sudo docker login 
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: 
Password: 

Login Succeeded
$ sudo docker images 
REPOSITORY                      TAG                 IMAGE ID            CREATED             SIZE
testimage                       001                 b09b0ff6c900        44 minutes ago      171MB
envoyproxy/envoy-build-ubuntu   my_tag              74b2f51293f9        4 days ago          3.12GB
hello-world                     latest              fce289e99eb9        6 days ago          1.84kB
ubuntu                          xenial              b0ef3016420a        9 days ago          117MB
ubuntu                          latest              1d9c17228a9e        9 days ago          86.7MB
$ sudo docker tag b09b0ff6c900 leopoldsga/testrepo:001
$ sudo docker push leopoldsga/testrepo:001
The push refers to repository [docker.io/leopoldsga/testrepo]
6c8b6628790e: Pushed 
2ee5bff9967a: Pushed 
2c77720cf318: Mounted from library/ubuntu 
1f6b6c7dc482: Mounted from library/ubuntu 
c8dbbe73b68c: Mounted from library/ubuntu 
2fb7bfc6145d: Mounted from library/ubuntu 
001: digest: sha256:6b1e2189d605bb493b8ec51e60987432f0fe5407ced60ac63cc67d7335793b98 size: 1574
```

![](/images/docker/push.jpg)

# Reference

[Docker Guide](https://www.tutorialspoint.com/docker/docker_quick_guide.htm)
