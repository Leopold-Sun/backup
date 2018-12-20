---
title: An biref introduction to FD.io VPP
date: 2018-12-17 14:12
tags: [VPP]
---

### Used links

- [What is VPP](https://wiki.fd.io/view/VPP/What_is_VPP%3F)
- [FD.io VPP](https://wiki.fd.io/view/VPP)
- [知乎](https://zhuanlan.zhihu.com/p/40049446)
- [SRI/OV DPDK](https://www.metaswitch.com/blog/accelerating-the-nfv-data-plane)
- [FD.io Takes Over VPP](https://www.metaswitch.com/blog/fd.io-takes-over-vpp)

### Preliminaries

#### Include DPDK

- Referrence:[SRIOV DPDK](https://www.metaswitch.com/blog/accelerating-the-nfv-data-plane)

> *The Data Plane Development Kit includes memory, buffer and queue managers, along with a flow classification engine and a set of poll mode drivers. Similar to the Linux New API (NAPI) drivers, it is the DPDK poll mode that performs the all-important interrupt mitigation that, as we know from SR-IOV, is critical to increasing the overall performance of the application. Operating in Polled Mode during periods of high traffic volume, the kernel periodically checks the interface for incoming packets, rather than waiting for an interrupt. To prevent "poll-ution" (yes, I just made that up to describe an excess of polls when no data is waiting for retrieval) while not leaving packets waiting when they do arrive (thereby increasing latency and jitter) DPDK can switch to interrupt mode when the flow of incoming packets falls below a certain threshold.*
> *With the queue, memory and buffer managers, DPDK can also implement zero-copy DMA into large first in, first out (FIFO) ring buffers located in user space memory, a process akin to PF_RING. That, again, dramatically improves overall packet acquisition performance by not only enabling faster capture but smoothing-out bursty inbound traffic, allowing the application to handle the packets more consistently and therefore more efficiently. Plus, if the guest CPU gets busy with applications processing, it can leave the packets in the buffer a little longer without the fear of those packets being discarded. Naturally, this buffer -- along with the poll/interrupt thresholds -- needs to be managed closely with latency-sensitive applications such as voice.*

![The FIFO Ring Buffer](https://www.metaswitch.com/hs-fs/hubfs/accelerating-the-NFV-data-plane-FIFO-ring.png?t=1451591795958&width=750&height=212&name=accelerating-the-NFV-data-plane-FIFO-ring.png)
![DPDK Accelerated OVS Within User Space](https://www.metaswitch.com/hubfs/accelerating-the-NFV-data-plane-ovs-micro-mega-flow-DPDK.png?t=1451591795958)
![Data plane acceleration testing results and comparisons, courtesy of HPE](https://www.metaswitch.com/hs-fs/hubfs/accelerating-the-NFV-data-plane-hp-tests-results.png?t=1451591795958&width=750&height=222&name=accelerating-the-NFV-data-plane-hp-tests-results.png)

### Vector Packet Processing

> *VPP uses vector processing as opposed to scalar processing. Scalar packet processing refers to the processing of one packet at a time. That older, traditional approach entails processing an interrupt, and traversing the call stack (a calls b calls c... return return return from the nested calls... then return from Interrupt). That process then does one of 3 things: punt, drop, or rewrite/forward the packet.*

#### 标量包处理的弊端

- I cache 抖动
- 每个包都会引入一组相同的cache未命中（标量工作方式使cache不断的被擦除加载）
- 增大cache容量是“唯一”的优化方式

![scalar packet processing graph](https://www.metaswitch.com/hs-fs/hubfs/Blogs/fd-io-pre-vpp-forwarding_graph.png?width=800&height=497&name=fd-io-pre-vpp-forwarding_graph.png)

#### 矢量包处理

- 解决I cache抖动问题 (L1 I cache 大约32K，D cache大约32K)
- 缓和了具有依赖性的读操作的延迟问题（该延迟可以通过pre-fetching方式消除）
- 解决stack depth/D cache misses on stack address

![Vector processing of IPv4 packets using DPDK](https://www.metaswitch.com/hs-fs/hubfs/Blogs/fd-io-vpp-forwarding_graph.png?width=800&height=496&name=fd-io-vpp-forwarding_graph.png)

#### vector processing procedure

> It could be referred as a circuit or cycle

- grab all available packets from the device RX ring
- form a "frame" (vector) that consists of packet indices in RX order
- run the packets through a directed graph of nodes
- return to the RX ring

#### Use case: vpp as a vSwitch/vRoute

![vSwitch](https://wiki.fd.io/images/1/15/VPP_App_as_vSwitch_with_local_programmability_x260.jpg)

#### Local Programmability

![local programmability](https://img-blog.csdn.net/20160421141931994)

- 通信行为
local环境下 external app 通过 a low level API 建立与 vpp app 之间的通信，合理性能可达 500k routes/sec。

- 通信方式
shared memory/message queue。
> *The implementation is on a local on a box or container. All CLI tasks can be done through API calls.*

