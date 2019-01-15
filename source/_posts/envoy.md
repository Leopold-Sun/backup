---
title: Introduction to Envoy
date: 2018-12-27 13:03:06
tags: [Envoy]
categories: [Envoy]
description:
keywords: envoy
---

<center>*The network should be transparent to applications. When network and application problems do occur it should be easy to determine the source of the problem.* </center>
>> [Envoy](https://www.envoyproxy.io/docs/envoy/latest/intro/what_is_envoy)

<!-- more  -->

# Envoy 

## What is Envoy?

> Envoy is an L7 proxy and communication bus designed for large modern service oriented architectures.

### Related Concepts

> [gRPC](https://grpc.io/docs/)
> [Envoy gRPC](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/grpc)
> [data plane api](https://github.com/envoyproxy/data-plane-api/blob/master/API_OVERVIEW.md)

## High-level features provided by Envoy

- Out of process architecture (非入侵式架构)
> `Envoy`是和应用服务并行运行的，透明地代理应用服务发出/接收的流量。应用服务只需要和`Envoy`通信，无需知道其他微服务应用在哪里。
- Modern C++11 code base
- L3/L4 filter architecture (3/4层过滤器)
> L3、L4代理是`Envoy`的核心，使用`pluggable filter chain mechanism`插件式过滤系执行TCP、UDP任务，如TCP转发、TLS认证。
- HTTP L7 filter architecture (7层过滤器)
> `Envoy`内置`http_connection_manager`（可以通过系列化http过滤器执行http协议层面的任务）。
- First class HTTP/2 support （支持http/2）
- HTTP L7 routing
- gRPC support
- MongoDB L7 support
- DynamoDB L7 support
- Service discovery and dynamic configuration
- Health checking
- Advanced load balancing
- Front/edge proxy support
- Best in class observability

### Network (L3/L4) Filters

The L3/L4 filters are the core of Envoy connection handling, which is comprised of three types of network filters:
- Read: Envoy invokes `Read Filter` when receiving data from a downstream host connection.
- Write: Envoy invokes `Write Filter` when sending  data to a downstream host connection.
- Read/Write: Envoy invokes `Read/Write Filter` when receiving and sending data from/to a downstream host connection at a same time.

## Teminology

`Host`: logical network application capable of network communication
`Downstream`: A downstream host connects to Envoy, sends requests, and receives responses.
`Upstream`: An upstream host receives connections and requests from Envoy and returns responses.
`Listener`: A named network location(e.g., port,unix domain socket, etc) that can be connected to by downstream clients.Envoy exposes one or more listeners that downstream hosts connect to.是Envoy监听的一个地址。
`Cluster`: A group of logically similar upstream hosts that Envoy connects to.Envoy通过[service discovery](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/service_discovery#arch-overview-service-discovery)发现集群成员;通过[active health checking](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/health_checking#arch-overview-health-checking)决定集群成员的健康状态;Envoy通过[load balancing policy](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/load_balancing/overview#arch-overview-load-balancing)据定将request route给那一个集群成员。
`Mesh`:  A group of hosts that coordinate to provide a consistent network topology.`Envoy Mesh`表示一组Envoy代理，这组代理为一个分布式系统（包含不同服务与应用）形成一个`messgae passing substrate`信息传输层。
`Runtime configuration`: Out of band realtime configuration system deployed alongside Envoy.修改envoy配置且不需要重启envoy或修改前（primary）配置以使之生效。
`Http Route Table`: HTTP 的路由规则，例如请求的域名，Path符合什么规则，转发给哪个 Cluster。

## Threading Model —— SPMT Model

> Envoy uses <mark>a single process with multiple threads architecture</mark>. A single master thread controls various sporadic coordination tasks while some number of worker threads perform listening, filtering, and forwarding. Once a connection is accepted by a listener, the connection spends the rest of its lifetime bound to a single worker thread. This allows the majority of Envoy to be largely single threaded (embarrassingly parallel) with a small amount of more complex code handling coordination between the worker threads. Generally Envoy is written to be 100% non-blocking and for most workloads we recommend configuring the number of worker threads to be equal to the number of hardware threads on the machine.——[Envoy Threading Model](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/threading_model)

## Listeners

- 当前Envoy仅支持TCP Listeners
- 推荐每台机器运行一个Envoy进程以简化操作和实现单源数据

# Envoy as a Front-Proxy

## Architecture

- ![](/images/envoy/front-envoy-archi.jpg)


-------------------------------

# References

- [yandy在掘金](https://juejin.im/post/5ad6fb06518825364001f619)
- [Envoy Configuration Reference](https://www.envoyproxy.io/docs/envoy/latest/configuration/configuration)
- [Getting started with Lyft Envoy for microservices resilience](https://www.datawire.io/envoyproxy/getting-started-lyft-envoy-microservices-resilience/)

