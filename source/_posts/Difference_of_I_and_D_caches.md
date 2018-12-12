---
title: Differences between I & D - cache
date: 2018-12-12 21:25
tags: [hardware architecture]
---

### Preliminaries

#### L1 and L2 cache 

> From [Webopedia](https://www.webopedia.com/)
> L1、L2 cache are memory caches using high speed SRAM
> *Some memory caches are built into the architecture of microprocessors. The Intel 80486 microprocessor contains an 8K memory cache, and the Pentium has a 16K cache. Such internal caches are often called Level 1 (L1) caches. Most modern PCs also come with external cache memory, called Level 2 (L2) caches. These caches sit between the CPU and the DRAM. Like L1 caches, L2 caches are composed of SRAM but they are much larger.*

> From [Wikipedia](https://en.wikipedia.org/wiki/)
> *A CPU cache is a hardware cache used by the central processing unit (CPU) of a computer to reduce the average cost (time or energy) to access data from the main memory. A cache is a smaller, faster memory, closer to a processor core, which stores copies of the data from frequently used main memory locations. Most CPUs have different independent caches, including instruction and data caches, where the data cache is usually organized as a hierarchy of more cache levels (L1, L2, L3, L4, etc.). Almost all current CPUs with caches have a split L1 cache. They also have L2 caches and, for larger processors, L3 caches as well. The L2 cache is usually not split and acts as a common repository for the already split L1 cache. Every core of a multi-core processor has a dedicated L2 cache and is usually not shared between the cores. The L3 cache, and higher-level caches, are shared between the cores and are not split.*

#### Disk caching

> Using conventional main memory, disk caching runs as memory buffer of disk. And *accessing a byte of data in RAM can be thousands of times faster than accessing a byte on a hard disk*

#### Smart caching 

> A technique named smart caching is used to improve _hit rate_ of caches.

###  I(instruction) cache and D(data) cache

#### Differences

- behavior

> I cache 大多顺序取值，会在分支指令跳转。指令行为包含读与refill，但是没有写。
> D cache 数据行为包含读和写。

- design

> *union cache，同时需要数据和指令的访问，端口上是很难实现的。在流水线的主干上,采用分离的icache和dcache. 非主干的L2 cache,从容量的角度考虑采用union的方式。*
