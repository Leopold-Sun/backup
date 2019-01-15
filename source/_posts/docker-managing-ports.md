---
title: docker - managing ports
tags: [docker]
categories: [docker]
date: 2019-01-07 17:28:41
---

> Using docker to run particular applications or services usually requires developer to map one specific port number of the container to the port number of the Docker host.

# Example form [Docker Guide](https://www.tutorialspoint.com/docker/docker_quick_guide.htm)

## Docker pull jenkins

```bash
$ sudo docker pull jenkins
```

<!-- more -->

## Find ports exposed by the container

### Used command

```bash
$ sudo docker inspect {Container ot Image}	# this command will return the low-level information of the image or container in JSON format.
```

### Example

```sh
$ sudo docker inspect jenkins
[
    {
        "Id": "sha256:cd14cecfdb3a657ba7d05bea026e7ac8b9abafc6e5c66253ab327c7211fa6281",
        "RepoTags": [
            "jenkins:latest"
        ],
        "RepoDigests": [
            "jenkins@sha256:eeb4850eb65f2d92500e421b430ed1ec58a7ac909e91f518926e02473904f668"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2018-07-17T16:20:34.183816595Z",
        "Container": "a3e3890f6333066d464113032a93622a8a12305fa1cf7a29e57ad29cbde66a19",
        "ContainerConfig": {
            "Hostname": "a3e3890f6333",
            "Domainname": "",
            "User": "jenkins",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "50000/tcp": {},
                "8080/tcp": {}
            },
```

#### ExposedPorts

- 50000/tcp
- 8080/tcp

#### Port Mapping

##### Command

```bash
$ sudo docker run -p 8080:8080 -p 50000:50000 jenkins
```

##### Discription

- `p` for port
- left side of `:` is the port number of docker host port
- right side of `:` is the docker container port number

##### Return Value

- From shell

```sh
Jan 08, 2019 3:10:42 AM hudson.model.UpdateSite updateData
INFO: Obtained the latest update center data file for UpdateSource default
Jan 08, 2019 3:10:42 AM hudson.model.UpdateSite updateData
INFO: Obtained the latest update center data file for UpdateSource default
Jan 08, 2019 3:10:42 AM hudson.WebAppMain$3 run
INFO: Jenkins is fully up and running
Jan 08, 2019 3:10:43 AM hudson.model.DownloadService$Downloadable load
INFO: Obtained the updated data file for hudson.tasks.Maven.MavenInstaller
--> setting agent port for jnlp
--> setting agent port for jnlp... done
Jan 08, 2019 3:10:45 AM hudson.model.DownloadService$Downloadable load
INFO: Obtained the updated data file for hudson.tools.JDKInstaller
Jan 08, 2019 3:10:45 AM hudson.model.AsyncPeriodicWork$1 run
INFO: Finished Download metadata. 10,672 ms
```

- From url: clientIP:8080

![](/images/docker/jenkins8080.jpg)

- From url: ClientIP:50000

```bash
Jenkins-Agent-Protocols: JNLP-connect, JNLP2-connect, JNLP4-connect, Ping
Jenkins-Version: 2.60.3
Jenkins-Session: 1358fde7
Client: {ClientIP returns from $ ip addr show, or you can use a local ip address like 127.0.0.1}
Server: 172.17.0.2
```

------------------------

# Reference

[Docker Guide](https://www.tutorialspoint.com/docker/docker_quick_guide.htm)
