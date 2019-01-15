---
title: 'envoy: front-proxy sandbox'
tags: [Envoy,doocker]
categories: [Envoy]
date: 2019-01-10 13:51:10
---

# Envoy Proxy简述

Envoy Proxy也可以充当service的`sidecar`，我们可以将自己的`service`与`envoy proxy sidecar`协同部署以实现微服务的功能。代理可以专注与实现路由、负载均衡、服务发现、熔断机制和超时重传等机制，而service只需专注与如何实现自身的功能而将流量的路由等工作`offload`至`sidecar`。

## 服务间通信

![](/images/envoy/service2service.svg)

## 集群内路由



<!-- more -->

# Front-proxy sandbox framework

![](/images/envoy/front.svg)

> 此沙箱部署一个前端Envoy反向代理与一些简单的services，而且每个service都被协同部署一个service envoy。Envoy代理、services各独占一个container，一起被部署于叫`envoymesh`的虚拟网格中。而实际上，所有的流量都是被route到Envoy Proxy of services。

## 端口映射

```bash
version: '2'
services:

  front-envoy:
    build:
      context: .
      dockerfile: Dockerfile-frontenvoy
    volumes:
      - ./front-envoy.yaml:/etc/front-envoy.yaml
    networks:
      - envoymesh
    expose:
      - "80"
      - "8001"
    ports:
      - "8000:80"
      - "8001:8001"

  service1:
    build:
      context: .
      dockerfile: Dockerfile-service
    volumes:
      - ./service-envoy.yaml:/etc/service-envoy.yaml
    networks:
      envoymesh:
        aliases:
          - service1
    environment:
      - SERVICE_NAME=1
    expose:
      - "80"

  service2:
    build:
      context: .
      dockerfile: Dockerfile-service
    volumes:
      - ./service-envoy.yaml:/etc/service-envoy.yaml
    networks:
      envoymesh:
        aliases:
          - service2
    environment:
      - SERVICE_NAME=2
    expose:
      - "80"

networks:
  envoymesh: {}
```

在`docker-compose.yml`配置文件中`80`端口被映射到`8000`端口，在`envoymesh`网格中，`front-envoy`暴露了`80`、`8001`，而这两个端口分别被映射到`8000`、`8001`端口。所以在这个沙箱中测试Envoy路由功能的时，有两种测试方式：
1. Container外部测试
`sudo curl -v localhost:8000/service/1`
2. Container内部测试
```bash
$ sudo docker-compose exec front-envoy /bin/bash
$ curl localhost:80/service/1
$ curl localhost:8001/stats
$ curl localhost:8001/server_info
```

# Docker-compose 启动 frontenvoy service

```bash
$ pwd
/home/pold/program/envoy/examples/front-proxy
$ sudo docker-compose up --build -d 
Creating network "frontproxy_envoymesh" with the default driver
Building front-envoy
Step 1/3 : FROM envoyproxy/envoy:latest
...
Step 2/3 : RUN apt-get update && apt-get -q install -y     curl
...
Step 3/3 : CMD /usr/local/bin/envoy -c /etc/front-envoy.yaml --service-cluster front-proxy
...
$ sudo docker-compose ps
          Name                        Command               State                            Ports                        
---------------------------------------------------------------------------------------------------------------------------
frontproxy_front-envoy_1   /docker-entrypoint.sh /bin ...   Up      10000/tcp, 0.0.0.0:8000->80/tcp, 0.0.0.0:8001->8001/tcp
frontproxy_service1_1      /bin/sh -c /usr/local/bin/ ...   Up      10000/tcp, 80/tcp                                     
frontproxy_service2_1      /bin/sh -c /usr/local/bin/ ...   Up      10000/tcp, 80/tcp
```

## 测Envoy路由

```bash
$ sudo curl -v localhost:8000/service/1
```

## 测试Envoy负载均衡
多次向service1发送请求，前端Envoy会将请求通过负载均衡发给三个service1服务:

```bash
$ sudo docker-compose scale service1=3
$ sudo curl -v 127.0.0.1:8000/service/1
$ sudo curl -v 127.0.0.1:8000/service/2
```

## 进入容器开启curl

```bash
$  sudo docker-compose exec front-envoy /bin/bash
```

## 通过Admin管理接口查看统计信息

```bash
$ sudo docker-compose exec front-envoy /bin/bash
$ curl localhost:8001/stats
$ curl localhost:8001/server_info
```

----------------

# Referece

- [Front-Envoy Sandbox](https://www.envoyproxy.io/docs/envoy/latest/start/sandboxes/front_proxy)
- [docker-compose.yml](https://raw.githubusercontent.com/envoyproxy/envoy/master/examples/front-proxy/docker-compose.yml)

