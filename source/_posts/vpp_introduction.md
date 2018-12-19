---
title: An biref introduction to FD.io VPP
date: 2018-12-17 14:12
tags: [VPP]
---

### Used links

- [What is VPP](https://wiki.fd.io/view/VPP/What_is_VPP%3F)
- [FD.io VPP](https://wiki.fd.io/view/VPP)
- [知乎](https://zhuanlan.zhihu.com/p/40049446)
- [SRI/OV + DPDK](https://www.metaswitch.com/blog/accelerating-the-nfv-data-plane)

### Vector Packet Processing

> *VPP uses vector processing as opposed to scalar processing. Scalar packet processing refers to the processing of one packet at a time. That older, traditional approach entails processing an interrupt, and traversing the call stack (a calls b calls c... return return return from the nested calls... then return from Interrupt). That process then does one of 3 things: punt, drop, or rewrite/forward the packet.*

#### 标量包处理的弊端

- I cache 抖动
- 每个包都会引入一组相同的cache未命中（标量工作方式使cache不断的被擦除加载）
- 增大cache容量是“唯一”的优化方式

![scalar packet processing graph](https://pic2.zhimg.com/80/v2-e2d556c865f20015e281032d2d1a218d_hd.jpg)

#### 矢量包处理

- 解决I cache抖动问题 (L1 I cache 大约32K，D cache大约32K)
- 缓和了具有依赖性的读操作的延迟问题（该延迟可以通过pre-fetching方式消除）
- 解决stack depth/D cache misses on stack address

![Vector processing of IPv4 packets using DPDK](https://pic2.zhimg.com/80/v2-43be19e7b128e1d268aa2a0608d992e9_hd.jpg)

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

