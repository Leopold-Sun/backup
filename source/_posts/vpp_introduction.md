---
title: An biref introduction to FD.io VPP
date: 2018-12-17 14:12
tags: [VPP]
---

### Used links

- [What is VPP](https://wiki.fd.io/view/VPP/What_is_VPP%3F)
- [FD.io VPP](https://wiki.fd.io/view/VPP)

### Vector Packet Processing

> *VPP uses vector processing as opposed to scalar processing. Scalar packet processing refers to the processing of one packet at a time. That older, traditional approach entails processing an interrupt, and traversing the call stack (a calls b calls c... return return return from the nested calls... then return from Interrupt). That process then does one of 3 things: punt, drop, or rewrite/forward the packet.*

#### 标量包处理的弊端

- I cache 抖动
- 每个包都会引入一组相同的cache未命中（标量工作方式使cache不断的被擦除加载）
- 增大cache容量是“唯一”的优化方式

#### 矢量包处理

- 解决I cache抖动问题 (L1 I cache 大约32K，D cache大约32K)
- 缓和了具有依赖性的读操作的延迟问题（该延迟可以通过pre-fetching方式消除）
- 解决stack depth/D cache misses on stack address

#### vector processing procedure

> It could be referred as a circuit or cycle

- grab all available packets from the device RX ring
- form a "frame" (vector) that consists of packet indices in RX order
- run the packets through a directed graph of nodes
- return to the RX ring

#### Use case: vpp as a vSwitch/vRoute

![Local Programmability](https://wiki.fd.io/images/1/15/VPP_App_as_vSwitch_with_local_programmability_x260.jpg)
