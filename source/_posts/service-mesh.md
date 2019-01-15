---
title: Introduction to Service Mesh
date: 2019-01-03 16:08:40
tags: [Envoy]
categories: [Envoy]
---

# Causality of Service Mesh

对于简单的、单一规模的系统，解决其日渐增大的复杂度的首选方法是尽可能多地减少`remote interactions`远程交互，因为处理分布式问题最安全的措施就是避免分布式，即使中心式的系统架构意味着`duplicated logic & data across various system`。

But……，是的。很多时候，人们为了交流顺利会提前阐述完整或者必须的前提(preliminary),而真正的观点或重要的事情都会放在<mark>BUT</mark>之后展开。

当服务的形式不再局限与单物理机和单一机柜、机房中，被拓展至云端，而且为了开发、维护等复杂度的降低而被切分为`microservices`后，用Gateway的方式优化微服务架构对微服务数量庞大的架构来说可不是首选方案。~~那么，service mesh出现了。~~

<!-- more -->
---------------
# What is Network Computers?

> [Pattern: Service Mesh](http://philcalcado.com/2017/08/03/pattern_service_mesh.html)

1. If two computers talk to each other.
	![](/images/service-mesh/NC-1.png)
2. Oversimplified view on two services talking to each other for some partiicular goals.
	![](/images/service-mesh/NC-2.png)
	> 为保证网络通信QoS，需要明确机器发现机制、同步连接处理、间接访问、路由、加密等实现细节。
	> 以<mark>flow control</mark>为例，服务器发送的包不会超出下游服务器的处理容量。
3. Applications or services logic contains flow control
	![](/images/service-mesh/NC-3.png)
4. Flow controls as standards like TCP/IP that are extracted to the underlying network layer
	![](/images/service-mesh/NC-4.png)

------------------
# What about Microservices?

> 在传统模式下，如果微服务之间要进行通信，那么程序需要自己处理各种通信的细节，这就包括服务发现、熔断机制、超时重试和 tracing 等功能。这些功能通常实现为与某种编程语言相关的 library，这也导致了这样的 library 无法在不同的编程语言之间共享。[senlinzhan](http://senlinzhan.github.io/2017/12/25/envoy/)

逐渐增多的节点与状态链接，工业界的网络系统发展包括`fine-grained distributed agents and objects`、`Service-Oriented Architectures`。相较而言，微服务的架构比这二者分布的程度更高，也需要考虑一下几种关键因素
- Rapid provisioning of compute resources
- Basic monitoring
- Rapid deployment
- Easy to provision storage
- Easy access to the edge
- Authentication/Authorisation
- Standardised RPC

## 以 Service Discovery 与 Circuit Breakers 为例

此二者皆是用于提高分布式系统的弹性（resiliency）和上述的几个挑战。

### 什么是 Service Discovery
> Service discovery is the process of automatically finding what instances of service fulfill a given query, e.g. a service called <mark>Teams</mark> needs to find instances of a service called <mark>Players</mark> with the attribute <mark>environment</mark> set to <mark>production</mark>. [Pattern: Service Mesh](http://philcalcado.com/2017/08/03/pattern_service_mesh.html)

在单一化的架构中，服务发现是一个由DNS、LB、端口约定（Http-8080）完成的简单task。

而分布式场景下，服务发现可能因为客户端的LB、异构环境、地理位置等因素而变得复杂。

### 什么是 Circuit Breakers
> Circuit breakers are a pattern catalogued by Michael Nygard in his book **Release It**.
> The basic idea behind the circuit breaker is very simple. You wrap a protected function call in a circuit breaker object, which monitors for failures. Once the failures reach a certain threshold, the circuit breaker trips, and all further calls to the circuit breaker return with an error, without the protected call being made at all. Usually you’ll also want some kind of monitor alert if the circuit breaker trips. [martinfowler](https://martinfowler.com/bliki/CircuitBreaker.html)

### 前期微服务架构
![](/images/service-mesh/MS-1.png)
与前期的网络通信系统类似，服务对于请求的处理逻辑全部交于服务开发者来做，包含上面列的几个关键挑战因素。

### Netflix/Twitter/SoundCloud microservice architecture
![](/images/service-mesh/MS-2.png)
其一，随着分布的服务数量增多，其可靠性也逐渐降低;其二，限制了微服务的开发工具、语言和运行时;其三，维护成本大。

### Extract the complicated features form service logic to the underlying platform
![](/images/service-mesh/MS-3.png)
解放开发者以专注与服务基本逻辑。但将这些服务特征放入协议栈是不可行的，而是以代理的形式被实现在服务逻辑之外。

### Here comes the sidecar concept
![](/images/service-mesh/MS-4.png)
> Use cases: Linkerd、Envoy

既然不同的service所实现的不同功能使得最终得到的不兼容、编程语言不同的`Libraries`之间无法共享，那么不如将这部分`libraries`剥离开作为独立的进程运行，而剥离开的`function library`也被通俗地称为`sidecar`。

#### service co-located with "sidecar"

![](/images/envoy/sidecar.png)

一般来说，可以选择将`sidecar`与`service`一起部署，比如放到同一个`container`中，那么该`service`的所有流量（包含`ingress flow`和`egress flow`）都交由`sidecar`代理。在此基础上，也可以为`sidecar`添加服务发现、熔断机制和超时重传等功能。

------------------
# What is Service Mesh?
![](/images/service-mesh/SM-1.png)

> The term <mark>service mesh</mark> is used to describe the network of microservices that make up such applications and the interactions between them. As a service mesh grows in size and complexity, it can become harder to understand and manage. Its requirements can include discovery, load balancing, failure recovery, metrics, and monitoring. A service mesh also often has more complex operational requirements, like A/B testing, canary releases, rate limiting, access control, and end-to-end authentication. [lstio-concepts](https://istio.io/docs/concepts/what-is-istio/)
> A service mesh is a dedicated infrastructure layer for handling service-to-service communication. It’s responsible for the reliable delivery of requests through the complex topology of services that comprise a modern, cloud native application. In practice, the service mesh is typically implemented as an array of lightweight network proxies that are deployed alongside application code, without the application needing to be aware. 
The concept of the service mesh as a separate layer is tied to the rise of the cloud native application. In the cloud native model, a single application might consist of hundreds of services; each service might have thousands of instances; and each of those instances might be in a constantly-changing state as they are dynamically scheduled by an orchestrator like Kubernetes. Not only is service communication in this world incredibly complex, it’s a pervasive and fundamental part of runtime behavior. Managing it is vital to ensuring end-to-end performance and reliability.[What’s a service mesh? And why do I need one?](https://blog.buoyant.io/2017/04/25/whats-a-service-mesh-and-why-do-i-need-one/)

![](/images/service-mesh/SM-2.png)

## Transfer microservice to service mesh
![](/images/service-mesh/SM-3.png)
网络服务依然在代理之间进行通信，但是<mark>Control Plane</mark>知道所有`proxy insatnces`的信息。通过代理之间的合作，可以实现一些对数据流的观察数据，比如`access control`、`metrics collection`。
![](/images/service-mesh/SM-4.png)

----------------------
# References

- [what is lstio](https://istio.io/docs/concepts/what-is-istio/)
- [Pattern: Service Mesh](http://philcalcado.com/2017/08/03/pattern_service_mesh.html)
- [Service Mesh 及其主流开源实现解析](https://liudanking.com/tag/envoy/)
- [What’s a service mesh? And why do I need one?](https://blog.buoyant.io/2017/04/25/whats-a-service-mesh-and-why-do-i-need-one/)
- [Envoy Proxy 与微服务实践](http://senlinzhan.github.io/2017/12/25/envoy/)
