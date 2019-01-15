---
title: docker installation
tags: [docker]
categories: [docker]
date: 2019-01-07 11:26:02
---

# Installing Docker on Ubuntu 18.04 LTS

## Check Linux kernel version

> Because <mark>[Docker](https://www.tutorialspoint.com/docker/docker_quick_guide.htm)</mark> is only designed to run on linux kernel version 3.8 and higher.

```bash
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 18.04.1 LTS
Release:	18.04
Codename:	bionic

$ uname -a	# return linux system information
Linux SunGuoao 4.15.0-43-generic #46-Ubuntu SMP Thu Dec 6 14:45:28 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
```

<!-- more -->

## Update your OS with the latest packages

```bash
$ sudo apt-get update	# `sudo` for root access, and `update` for all packages being updated.
```

## Install docker certificate for downloading its packages

```bash
$ sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
Reading package lists... Done
Building dependency tree
Reading state information... Done
ca-certificates is already the newest version (20180409).	# It turns out that this options is done before.
apt-transport-https is already the newest version (1.6.6).
The following package was automatically installed and is no longer required:
  libssl-doc
Use 'sudo apt autoremove' to remove it.
0 upgraded, 0 newly installed, 0 to remove and 4 not upgraded.
```

## Add apt source in `/etc/apt/source.list.d/docker.list`, which adds the relevant site to this docker.list for the apt package manager. 

```bash
deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable
```

## Add GPG key for encrypted downloading

```bash
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

## Update apt and install docker/docker-compose/docker-machine

```bash
$ sudo apt-get update	# ensure all packages on the local system are up to date
$ sudo apt-get install –y docker-engine	# The Docker-engine is the official package from the Docker Corporation for Ubuntu-based systems.
$ sudo curl -L https://github.com/docker/machine/releases/download/v0.13.0/docker-machine-`uname -s`-`uname -m` > /usr/local/bin/docker-machine
$ sudo chmod +x /usr/local/bin/docker-machine
```
----------------------------------------

# More options of Docker

## Docker Info

> This command returns more details of docker running on your system.

```bash
$ sudo docker info
```

## check docker version

```bash
$ sudo docker version 
Client:
 Version:           18.09.0
 API version:       1.39
 Go version:        go1.10.4
 Git commit:        4d60db4
 Built:             Wed Nov  7 00:49:01 2018
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          18.09.0
  API version:      1.39 (minimum version 1.12)
  Go version:       go1.10.4
  Git commit:       4d60db4
  Built:            Wed Nov  7 00:16:44 2018
  OS/Arch:          linux/amd64
  Experimental:     false
```

---------------

# Reference

-[linux wiki](https://linuxwiki.github.io/Services/Docker.html)

# References

- [Docker Quick Guide](https://www.tutorialspoint.com/docker/docker_quick_guide.htm)
