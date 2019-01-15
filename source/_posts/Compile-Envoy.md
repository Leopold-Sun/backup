---
title: Compile Envoy
date: 2019-01-03 15:16:18
tags: [Envoy]
categories: [Envoy]
---

# Git clone

```bash
$ git clone https://github.com/envoyproxy/envoy.git
```
<!-- more -->

# Install docker

```bash
$ sudo apt update
$ sudo apt install apt-transport-https ca-certificates curl software-properties-common
$ sudo gedit /etc/apt/sources.list.d/docker.list     # add the following content: deb [arch=amd64] https://download.docker.com/linux/ubuntu_bionic_stable
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo apt update​
$ sudo apt install docker-ce
$ docker -v
```

# Compile envoy

> [Envoy Guide](https://github.com/envoyproxy/envoy/blob/master/ci/README.md)
The build image defaults to envoyproxy/envoy-build-ubuntu

```bash
$ sudo ./ci/run_envoy_docker.sh './ci/do_ci.sh bazel.dev'
```

# Testing changes to the build image as a developer

> This build the Ubuntu based envoyproxy/envoy-build-ubuntu image, and the final call will run against your local copy of the build image.

```bash
$ export DISTRO=ubuntu
$ cd ci/build_container
$ LINUX_DISTRO="${DISTRO}" CIRCLE_SHA1=my_tag ./docker_build.sh # Wait patiently for quite some time
```

## Return value

```bash
Successfully built 74b2f51293f9
Successfully tagged envoyproxy/envoy-build-ubuntu:my_tag
```

