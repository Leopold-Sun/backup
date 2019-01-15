---
title: Introduction to DMA solutions
date: 2017-10-10 14:08
tags: [NFV]
categories: NFV
---

### References

- [What is VPP](https://wiki.fd.io/view/VPP/What_is_VPP%3F)
- [FD.io VPP](https://wiki.fd.io/view/VPP)
- [知乎](https://zhuanlan.zhihu.com/p/40049446)
- [SRI/OV DPDK](https://www.metaswitch.com/blog/accelerating-the-nfv-data-plane)
- [FD.io Takes Over VPP](https://www.metaswitch.com/blog/fd.io-takes-over-vpp)
- [Introduction to DPDK: Architecture and Principles](https://blog.selectel.com/introduction-dpdk-architecture-principles/)

<!--more -->

-------------------------

### DPDK

- Referrence:[SRIOV DPDK](https://www.metaswitch.com/blog/accelerating-the-nfv-data-plane)

**Features**
- leverages the features of the underlying Intel hardware, it comprises a set of lightweight software data plane libraries and optimized NIC drivers that can be modified for specific applications

> *The Data Plane Development Kit includes memory, buffer and queue managers, along with a flow classification engine and a set of poll mode drivers. Similar to the Linux New API (NAPI) drivers, it is the DPDK poll mode that performs the all-important interrupt mitigation that, as we know from SR-IOV, is critical to increasing the overall performance of the application. Operating in Polled Mode during periods of high traffic volume, the kernel periodically checks the interface for incoming packets, rather than waiting for an interrupt. To prevent "poll-ution" (yes, I just made that up to describe an excess of polls when no data is waiting for retrieval) while not leaving packets waiting when they do arrive (thereby increasing latency and jitter) DPDK can switch to interrupt mode when the flow of incoming packets falls below a certain threshold.*
> *With the queue, memory and buffer managers, DPDK can also implement zero-copy DMA into large first in, first out (FIFO) ring buffers located in user space memory, a process akin to PF_RING. That, again, dramatically improves overall packet acquisition performance by not only enabling faster capture but smoothing-out bursty inbound traffic, allowing the application to handle the packets more consistently and therefore more efficiently. Plus, if the guest CPU gets busy with applications processing, it can leave the packets in the buffer a little longer without the fear of those packets being discarded. Naturally, this buffer -- along with the poll/interrupt thresholds -- needs to be managed closely with latency-sensitive applications such as voice.*

![The FIFO Ring Buffer](https://www.metaswitch.com/hs-fs/hubfs/accelerating-the-NFV-data-plane-FIFO-ring.png?t=1451591795958&width=750&height=212&name=accelerating-the-NFV-data-plane-FIFO-ring.png)
![Linux with/without DPDK](https://blog.selectel.com/wp-content/uploads/2016/11/PR-3303.png)
![Intel DPDK Libraries and environment abstraction layer](https://camo.githubusercontent.com/0e41cf40c009ffe95ff2c920e210b9ed850900a5/68747470733a2f2f7777772e657665726e6f74652e636f6d2f6c2f41533536724c58745a4752494e4c6b615268525a65564d617338424b48664d74706b49422f696d6167652e706e67)

- DPDK bypassing kernel
Use Linux huge pages to pre-assign large memory region and directly DMA data form NICs to the pre-assigned memory region.

------------------------

### DMA software solutions

#### Generic (VM+hypervisor+L2 vSwitch)

![architectures differences for packet delivery in virtualized platform](https://datawine.github.io/img/2018-9-24-2.png)

- Packets will be copied to memory of the node which hosts the hypervisor;
- then be forwarded in L2 based on information such as full 5-tuple packet header to match the destination VM;
- and be delivered to NFs/VMs via L2 vSwitch.
- legacy application support 
- a general lack of reliance on “specialized” hardware
- extraordinarily high CPU utilization（ultimately reduces throughput and the number of VMs that can be realistically hosted on a single platform）


**Features**
- packet copy
> Packets will be copied to user memory of the peculiar application, which results in memory overheads that get in the way of achieving line-rate throughput.
- interrupt-driven (**generic communication behavior**)
> *When traffic is received by the NIC, an interrupt request (IRQ) is sent to the CPU, which then must stop what it’s doing to retrieve the data. As anyone who has kids will understand, the interruptions are endless -- reducing the CPU’s ability to perform its primary computational tasks. In virtualized hosts, this problem is exaggerated even further. Not only does the CPU core running the hypervisor get interrupted, but also, once it has identified what VM the packet belongs to (based on MAC or VLAN ID, for example), it must then send a second interrupt to the CPU core running the VM itself, telling it to come get its data. * ——[SIMON DREDGE](https://www.metaswitch.com/blog/accelerating-the-nfv-data-plane)

#### VMDq (Virtual Machine Device Queues)

![VM 3-ways](https://www.metaswitch.com/hs-fs/hubfs/accelerating-the-NFV-data-plane-vm-3-ways.png?t=1451591795958&width=750&height=316&name=accelerating-the-NFV-data-plane-vm-3-ways.png)

**Features**
- enable the hypervisor to assign a distinct queue in the NIC for each VM
- only the CPU core hosting the destination VM need be interrupted such that interrupts are more fairly load-balanced across all VM host cores
- packet is only touched once, as it is copied directly into the VM user space memory
- the sheer number of interrupts in data-plane-centric virtualized network functions will still have an incredibly detrimental effect on performance.

#### SR-IOV (Single Root - Input/Output Virtualization)

![VM 3-ways:Normal operation through bottleneck eradication (VMDq) and interrupt mitigation (SR-IOV)](https://www.metaswitch.com/hs-fs/hubfs/accelerating-the-NFV-data-plane-vm-3-ways.png?t=1451591795958&width=750&height=316&name=accelerating-the-NFV-data-plane-vm-3-ways.png)

DPDK支持SR-IOV，SR-IOV可在逻辑上划分一个NIC为多个vNIC，并将一个单独的基于PCI的NIC（称为“VF-Virtual Function”）暴露给每个VM。平台直接在NIC上执行L2交换，之后将数据copy到对应用户态的VM中。该模型需要NIC维持数据mapping和包头信息，而不是MAC地址（MAC地址不能用于路由，极大的降低了数据路由的灵活性）。但是同时NIC只能由一个进程使用（due to the fact that the PCI device is directly assigned to a guest），如果两个VMs共享一个VF，那么就需要一次数据复制才能满足两个VM的数据需求；更恶劣的是，有些数据需要经过一个VM的处理通过网卡传送出去再传送到另外一个VF再传递给目的NF。

**Features**
- hypervisor is responsible for carving out specific hardware resources in the NIC for each VM instance
- VF
> *A virtual function consists of a limited, lightweight, PCIe resource and a dedicated transmit and receive packet queue. Loading-up a VF driver on boot-up, each VM is assigned one of these virtual functions by the hypervisor.The VF in the NIC is then given a descriptor that tells it where the user space memory, owned by the specific VM it serves, resides. Once received on the physical port and sorted (again by MAC address, VLAN tag or some other identifier) packets are queued in the VF until they can be copied into the VMs memory location.* ——[SIMON DREDGE](https://www.metaswitch.com/blog/accelerating-the-nfv-data-plane)
- interrupt-free operation bestows demanded compute operations of VNFs on VM's CPU core.

##### PCI passthrough

> *PCI passthrough is the only fast thing, but only one VM can use it at any one time, due to the fact that the PCI device is directly assigned to a guest. PCI passthrough does have the advantage of being a pure software solution, unlike SR-IOV, which is based on specialized (if not standardized) hardware features. But I’m ignoring that as well.* ——[SIMON DREDGE](https://www.metaswitch.com/blog/accelerating-the-nfv-data-plane)

#### OVS (Open vSwicth)

![Intro to OVS](https://i.ytimg.com/vi/rYW7kQRyUvA/hqdefault.jpg)
![OVS Architecture](http://www.fiber-optic-transceiver-module.com/wp-content/uploads/2018/08/OpenvSwitch-architecture.jpg)

[Features](http://www.openvswitch.org/features/)

![OVS Circa 1.11, featuring the kernel-level Megaflow Cache together with the Microflow Cache.](https://www.metaswitch.com/hs-fs/hubfs/accelerating-the-NFV-data-plane-ovs-micro-mega-flow.png?t=1451591795958&width=604&name=accelerating-the-NFV-data-plane-ovs-micro-mega-flow.png)

- introduces concepts such as the Megaflow installing a secondary flow cache entry in the OVS kernel.
> *With OVS split between kernel and user space, an exact-match Microflow cache of recently forwarded packets, once identified and run through the pipeline in user space via a Netlink inter-process communication (IPC), is installed in the kernel space. The additional Megaflow cache includes a wildcard for each flow entry that matches on many packets, not just one. This prevents a wider range of traffic from having to make the performance-stifling IPC context switch between kernel and user space and hit the OVS daemon pipeline.* ——[SIMON DREDGE](https://www.metaswitch.com/blog/accelerating-the-nfv-data-plane)

- costly IPC
> *While it is claimed that, by OVS 2.1, this resulted in “ludicrous speed”5 enhancements, Megaflow’s ability to ‘accelerate’ OVS still depends on the characteristics and behavior of the packets and traffic patterns traversing it and therefore varies from application to application. Put simply, it either works or it doesn’t -- a level of unpredictability that doesn’t typically fly with those looking to develop and deploy commercial services. Either way, it still has that costly IPC, which is where the Data Plane Development Kit (DPDK) comes into play.*——[SIMON DREDGE](https://www.metaswitch.com/blog/accelerating-the-nfv-data-plane)
-----------------

### Discussion

All the above DMA techniques bypass everything between the interface and the VM, thereby *reducing the chance of provisioning errors and opening this data plane acceleration option up to the NFV management and orchestration (MANO) layer, the DMA techniques it employs still ultimately results in traffic bypassing the Hypervisor vSwitch.This raises questions about SR-IOV’s ability to support the portability, flexibility, QoS, complex traffic steering (including network service headers for service function chaining, if the VNFs are middleboxes) and the expected cloud network virtualization demands of NFV.* ([SIMON DREDGE](https://www.metaswitch.com/blog/accelerating-the-nfv-data-plane))

Apparently, SR-IOV and vSwicth can be used in parallel in a very single host and cloud environment.

> *SR-IOV is an excellent option for “virtualization,” or the implementation of a stand-alone virtualized appliance or appliances, and it’s highly desirable to have an architecture where high-traffic VNFs, routers or Layer 3-centric devices use SR-IOV while Layer 2-centric middleboxes or VNFs with strict intra-host east-west demands employ a vSwitch. *——[SIMON DREDGE](https://www.metaswitch.com/blog/accelerating-the-nfv-data-plane)

Using SR-IOV and vSwitch in parallel would increase layer management complexities and rule out the possibility of a common, efficient and flexible SDN host deployment infrastructure. 
