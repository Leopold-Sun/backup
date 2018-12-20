---
title: NetVM
date: 2018-12-20 12:06
tags: [NFV]
---

## NetVM

NetVM是一个虚拟化平台，支持使用商用硬件，以线性速率运行复杂的网络功能。NetVM的实现基于KVM和DPDK技术，利用DPDK的高速包处理性能，并且在此基础上增加一系列抽象，可以灵活创建、chain、平衡负载服务。 

NFV平台发展的主要目的是在VMs中支持运行基于软件的数据层，减弱NFs的硬件依赖性。因为SDN虽然支持基于软件的控制层，但是数据层依赖网络硬件。促进了支持基于软件数据层技术的发展。相比于中间件的巨大开销，一个高性能的NFV平台可以在不同节点上动态部署NFs，开销小。而且任意大小的数据包都可以以线性速率从一个VM转移到另一个VM。 

SDN也在为提升网络配置能力而进步，通过几个方面提高网络配置的灵活性：软件控制网络控制层，数据层实现依然依赖网络硬件，数据转移。但是SDN无法实现数据层很多类型NFs的动态管理，这也阻止了这些类型的NFs的虚拟化进程。NFV就是支持在VMs执行基于软件的数据层。 

### DPDK平台的优势

DPDK平台支持应用绕过内核直接访问NICs，具体实现是使用Linux的huge pages预先配置很大的内存区域，然后直接从NICs中DMA数据到预先配置的内存区域。 

![DPDK’s run-time environment over Linux](Leopold-Sun.github.io/images/DPDK-runtimr-over-linux.png)
 
DPDK有一个poll mode driver允许VM应用绕过内核直接访问硬件设备。 

但是即使DPDK实现了用户态高性能应用，但是没有实现一个完整的矿街，可以用来构建、使交互复杂的网络功能。除此之外，DPDK支持SR-IOV技术，SR-IOV可以在逻辑上划分一个NIC并将一个单独的基于PCI的NIC（称为“VF-Virtual Function”）暴露给每个VM。 

![DPDK uses per-port switching with SR-IOV](Leopold-Sun.github.io/blob/master/images/DPDK%20uses%20per-port%20switching%20with%20SR-IOV.png)

Fig. 2. DPDK uses per-port switching with SR-IOV, whereas NetVM pro- 
vides a global switch in the hypervisor and shared-memory packet transfer 
(dashed lines). 
- (a) SR-IOV. 
- (b) NetVM. 

不同于SR-IOV，NetVM支持无copy的灵活交换行性能。而且不像硬件中间件，能跟根据需求动态创建VMs满足网络服务要求，而且能够在VM之间提供性能隔离、简化配置。 

### NFV平台差异 

![Architectural differences for packet delivery in virtualized platform](Leopold-Sun.github.io/blob/master/images/Architectural%20differences%20for%20packet%20delivery%20in%20virtualized%20platform.png)

Fig. 4. Architectural differences for packet delivery in virtualized platform. 
> (a) Generic. 
> (b)SR-IOV 
> (c) NetVM. 
- 传统平台在NIC接收到数据时，复制到hypervisor中。或者是执行L2（或者更复杂的信息，基于full 5-tuple packet header）交换将数据包与VM匹配，经过VF传递给对应NFs/VMs。主要的特点是需要copy数据包，不论是复制or赋给Guest OS，最终数据都会被复制到用户态的应用，造成大的开销，妨碍线性吞吐量的实现。 

- SR-IOV平台。DPDK支持SR-IOV技术，SR-IOV可以在逻辑上划分一个NIC并将一个单独的基于PCI的NIC（称为“VF-Virtual Function”）暴露给每个VM。平台直接在NIC上执行L2交换，之后将数据copy到对应用户态的VM中。该模型需要NIC维持数据mapping和包头信息，而不是不能用于路由的MAC地址。极大的降低了数据路由的灵活性。如果两个VMs共享一个VF，那么就需要一次数据复制才能满足两个VM的数据需求；更恶劣的是，有些数据需要经过一个VM的处理通过网卡传送出去再传送到另外一个VF再传递给目的NF。 

- NetVM不依赖SR-IOV。允许hypervisor中的用户空间应用分析包并配置数据包的流向。通过共享内存Guest 用户空间应用读取数据。满足灵活交换和高性能的要求。 

## 背景 

**阻碍商用服务器实现网络流线性处理的因素有** 

- 网络流到达时间不可预测，常实现中断驱动作为数据到达的信号。但是因为如今处理器使用long pipelin、无序与预测执行机制、多层内存系统，都使得中断造成的CPU消耗过大。而且当包到达速率提升，服务吞吐量会极大降低。 

- 数据包被读进内核在被复制发送给用户应用，造成很大开销。DPDK就是针对此实现数据的零复制传递，而且绕过内核，用户应用直接访问NICs。具体实现是使用Linux的huge pages预先配置很大的内存区域，然后直接从NICs中DMA数据到预先配置的内存区域。 

**NetVM解决数据包复制问题**
 
> DPDK结合vSwitch的方法没能解决数据复制的局限性，也不能支持链上VM间的通信。NetVM支持状态、数据依赖的数据交换方式，高效的链上VMs通信，也实现了安全domians以隔离VM Groups。DPDK有一个poll mode driver允许VM应用绕过内核直接访问硬件设备。 

 

### 灵活的网络服务 

一些中间件，比如防火墙、代理或者一些上架的平台，都是作为一个路由器实现的，但是中间件的所有入流的目的MAC地址是相同的。 

但是经过hypervisor最初的包switching，NetVM可以提供非常复杂、动态（目的MAC、地址可变）的网络功能。可以基于shallow （基于header）或者深度（基于data）的包分析机制，利用数据流的鉴别来在VM间对包进行路由操作。 

NetVM的switch也能使用像VM负载、日间时间、动态配置的策略此类的状态型信息进行交换算法的控制。 

 

### 基于网络的VM 

一个NF可能由中间件与网络硬件结合构建而成。NetVM支持软件快速处理数据，使得“压缩软件片与OS依赖可能造成的影响”微乎其微，更可以简化同一server上运行的多进程的部署。也便于部署用户定义的NFs，因为VM的实例化过程非常的灵活，可配置大量的VMs。同时呢，VM的隔离就显得非常重要了。比起在裸机上跑服务，使用VM还能简化资源分配和提升性能隔离。 

 

### NFs的管理 

怎么才能实现随时、随地地将NFs部署、迁移到新的VMs中呢？ 

配合使用OpenFlow SDN Controller 和一个NFV 控制器，前者用于控制数据Flow，后者用于将决定何时、何处放置NFs。二者之所可以紧密写协作是因为，数据流往往都能够遍历多个NFs，也可能希望启用新的服务并重新进行数据流的处理。所以需要维护现存数据流与NFs的状态以便于协作处理。 

![Net VM platforms are distributed in the network (and/or data centers)](Leopold-Sun.github.io/blob/master/images/NetVM%20platforms%20are%20distributed%20in%20the%20network.png)

黑点代表一个维护多个NFs的NetVM平台，它们通过NFV Orchestrator （例如，the Nf-Vi interface deﬁned in ETSI ）与SDN Controller （OpenFlow协议）进行管理，NetVM通过secure channels提供两者的接口。可以将支持NetVM平台的servers灵活部署到流路径中需要额外功能的位置。 

例如，在边缘网络上安装检测DDoS攻击的网络功能比将所有可疑流量重定向到执行深度数据包检查的集中式“数据包清理程序”要高效得多。 

## 系统设计 

1[]()
Fig. 4. Architectural differences for packet delivery in virtualized platform. 
> (a) Generic. 
> (b)SR-IOV 
> (c) NetVM. 
- 传统平台在NIC接收到数据时，复制到hypervisor中。然周vSwitch执行L2（或者更复杂的信息，基于full 5-tuple packet header）交换将数据包与VM匹配，经过对应的vNIC传递（复制数据）给对应NFs/VMs的Guest OS，之后再将数据复制传递给用户空间的应用sockets。主要的特点是需要copy数据包，不论是复制or赋给Guest OS，最终数据都会被复制到用户态的应用，造成大的开销，妨碍线性吞吐量的实现。 

- SR-IOV平台。DPDK支持SR-IOV技术，SR-IOV可以在逻辑上划分一个NIC并将一个单独的基于PCI的NIC（称为“VF-Virtual Function”）暴露给每个VM。平台直接在NIC上执行L2交换，之后将数据copy到对应用户态的VM中（绕过内核、CPU）。该模型需要NIC维持数据mapping和包头信息，而不是维持不能用于路由的MAC地址，以降低灵活性（维持map、包头信息，有“固定”目的地传送数据）的代价最小化数据地转移。极大的降低了数据路由的灵活性。如果两个VMs共享一个VF，那么就需要一次数据复制才能满足两个VM的数据需求；更恶劣的是，有些数据需要经过一个VM的处理通过网卡传送出去再传送到另外一个VF再传递给目的NF。 

- NetVM不依赖SR-IOV。允许使用hypervisor中的用户态应用执行分析包并配置数据包的流向的任务。通过共享内存，Guest 用户空间应用可以读取需要的数据。满足灵活交换和高性能的要求。 

 

### 零复制数据（包）传递 

![](Leopold-Sun.github.io/blob/master/images/NetVM%20only%20requires%20a%20simple%20descriptor%20to%20be%20copied%20via%20shared.png)

NetVM可以将路由器、代理等复杂的网络服务固定在一个单一主机上，同时为了支持这些组件（服务）间的快速通信，NetVM设计实现两种通信通道以供数据转移： 

- 一小段共享内存区域。如上图标记1.2.3，hypervisor与VMs通过此段内存进行通信。 

- 大内存页。如上图标记a、b，供信任的VMs分组进行通信，特别是链上VMs可以直接读写此段共享内存页的数据。甚至可以扩展共享内存使得一条链上所有的VMs直接访问其中的数据流。 

有一个NetVM Core，是作为DPDK支持的用户应用运行的，轮询NIC后通过DMA直接将数据读入大内存页，此应用（or NetVM Core）可以根据如包头、可能的包内容、或VM负载数据等信息决定每个数据包的发送位置。在hypervisor与个体VM之间设置有一个ring buffer，NetVM之后将指向每个包的描述符插入到该ring buffer中，描述符中包含mbuf的位置（类似于Linux 内核中的sk_buff）、对应包的处理操作（通过NIC传输、舍弃、传递给另一个VM等）、包接收位置在大内存页中的偏移量（offset）和role number。因为本质上VM中运行的是NF（如路由器、代理、防火墙等），所以VM manager（也就是hypervisor）给每个VM分配一个“role number”以作为NF的ID。 

虽然不用复制数据，但是描述符必须要在guest和hypervisor之间进行复制，使得guest之后可以直接访问大内存页中的数据。guest应用在分析了相应的数据包之后，可以通知NetVM传递（forward）数据给另一个VM、NIC传输包。传递数据包（forward）只是重复了上述过程，NetVM将描述符复制到另一个VM的ring buffer中以使该VM可以处理，本质上被操作的数据包在大内存页中的位置是不变的，因为复制、转移的是描述符，但是数据本身可以被VM进行处理、修改。 

 

### 无锁设计 

- Situation
共享内存一般使用lock锁进行管理，使数据的访问操作串行化，但是不可避免的降低了性能、增大了通信开销。提供线性性能的网络的要求：包大小无关的前提下维持10Gbps的满吞吐量，一个包必须在67.2ns之内被处理掉。锁竞争导致上下文切换在ms级以上，增大了处理延迟，最终导致数十个包被丢弃。 

- Solution
并行队列（分布在特定的核上）。NIC（支持RSS）根据一个配置好的hash，将收到的包分配给却不同的流队列。只允许hypervisor中的DPDK线程与VM中的应用（数据消耗）线程这两个线程可以操作对应的共享队列（某个流队列、并行队列中的一个），因为此队列具有单一的生产者和消费者，所以不需要对响应共享内存区域进行额外的数据同步工作，所以无需实现锁机制。 
此方法的伸缩性也可以得到保证，只需要创建额外的队列（被两个线程共同管理）就可以了。由于NIC支持RSS，NIC可以对数据流进行合理分配（给众多queues）以保证负载平衡。 
大内存页也不需要设计锁来进行数据同步，因为同一时间一个数据包的控制权只在一个VM上，因为其描述符只存在一个VM的ring buffer中。 

![](Leopold-Sun.github.io/blob/master/images/Lockless%20and%20NUMA-Aware%20Queue%20Thread%20Management.png)

每个圈代表一个核。hypervisor中的每个核从NIC中收包、将描述符放入队列尾部；guest os线程从队列头部读取描述符、处理数据包、放入出队列；hypervisor从出队列尾部读取描述符；NIC发送对应的包。此线程、队列分离能够保证同一时刻是有一个实体访问数据。 

### NUMA感知 

- Situation
多处理器系统都有NUMA特性：内存访问时间依赖处理器的内存位置。应当避免不同sockets上的核访问映射到相同cache line的内存，会造成两个核间很大的cache无效的通信开销。所以多服务器的NUMA特性不能简单的忽略，否则会造成延迟敏感型任务（e.g. 网络处理）的很大的性能下降。 比如，3GHz Intel Xeon 5500 处理器的L3命中需要40个cycles，但是L3未命中的代价是201个cycles。所以若是两个独立sockets访问内存位置邻近的数据，性能下降5倍，因为cache line持续出现未命中的情况（无效情况），这是因为cache是基于空间的局部性原理的，由于被标记的内存字的邻近单元也有可能被使用，所以会把它们也一起加载到cache中去。因此一个cache的入口处存储的并不是单一的字，而是几个连续的字，它们被称为缓存行(cache line)。 

- Solution
使用NUMA感知模式的大内存页避免上述问题。 
当需要查找大内存页面的区域时，会将内存区域统一分配到所有的sockets中，从而从sockets本地的DIMM中分配总共（大页面大小/sockets数量）字节的内存。同时创建数量与sockets一致的收/发线程，每个线程只处理对应socket的内存页中的数据。Guest VMs中的线程创建后被固定到合适的socket上。保证不需要在sockets间传递cache line，数据被储存在本地内存bank中，要么被host访问，要么被guest访问。 

![](Leopold-Sun.github.io/blob/master/images/Lockless%20and%20NUMA-Aware%20Queue%20Thread%20Management.png)

上图表示了灰socket与白socket的管理。灰线程处理的数据不会转移到白线程中，提高内存访问速度，避免cache一致性开销（因为各线程负责自己对应的socket数据的处理）。 

上图还表示了NetVM处理数据的流水线。可以知道的是，不同VMs间数据的传递是要经过hypervisor的管理的（复制、传递描述符）。 

### Huge Page虚拟地址映射 

- Situation
单个大内存页是连续的大段内存区域。也可以根据需要重新配置内存页的大小，默认配置在路径“under/proc/meminfo”下。因为hypervisor的地址空间是guest不知的，guest只能依靠描述符所带的信息访问共享内存页的数据，由于描述符是hypervisor创建的，而且描述符带的位置信息只是关于内存页的偏移量（offset），所以数据包地址（由NIC经过DMA读进内存）只对hypervisor有用，hypervisor需要翻译地址使得guest能够访问共享内存区域。此外，线性数据处理需要尽可能快的地址查找。 

- Solution
如下图，NetVM将大内存页以连续的区域映射给guests。 

![](Leopold-Sun.github.io/blob/master/images/The%20huge%20pages%20spread%20across%20the%20host%E2%80%99s%20memory.png)

NetVM模拟PCI设备（实现中有讲述）将大内存页映射到guests，guest通过驱动器轮询虚拟PCI设备将连续的内存区域映射到自己的用户空间。但以上的机制是不会暴露给未信任guests的，未信任guests通过hypervisor访问常规的网络接口。 

- Situation
NetVM DMA数据到大内存页，得到一个描述符与在hypervisor虚拟地址空间中的地址，这地址对guest都没有用。所以guest可能会遍历内存页组成的列表以确定内存页位置，但是这样的开销很大，每个包都需要经历这个过程。 

- Solution
NetVM使用bits操作与precomputed lookup table。 

#### 一个包的历程

NIC收包DMA到某个大内存页；创建的索引映射（index map）将包地址转换成大内存页索引（index，索引取自包地址的31-38位，前30位是其对应大内存页中的偏移量，38之后的位可以忽略）；根据IDMAP（h）值（该值存在数组HMAP [i]中）得到大内存页number。h是内存地址。 

#### 大内存页的基准地址计算

为得到包所属大内存页的起始地址（地址基准），使用一个累积（accumulated）的基准地址。如果所有大内存页大小相同，就不需要基准地址。但是，存在不同大小的大内存页，就需要持续跟踪基准地址的累积结果。HGIH (i)保存所有大内存页索引i的起始地址；剩余地址取自最后包地址的30位，使用函数LOW (a)=a&0x3FFFFFFF；计算模拟PCI中连续大内存页的偏移量使用函数OFFSET (p)=HIGH ( HMAP [ IDMAP (p) ] ) | LOW (p)。 

### 信任、不信任VMs

- Situation
恶意VM可能猜测到其他VMs数据位置，进而进行修改、丢弃操作。 

- Solution
信任、不信任VMs分组管理。VM创建之后分配到信任组，trust group决定其可以访问的内存区域。NetVM仅支持信任分类VMs，可能还会有更细致的分类方式。每个用户可以控制一个trust group。 

![](Leopold-Sun.github.io/blob/master/images/Lockless%20and%20NUMA-Aware%20Queue%20Thread%20Management.png)

每个VMs group有自己的内存区域，每个VM都配置一个ring buffer以供与hypervisor通信。 

### NetVM控制路 

NFs的通用部署应当由SDN或NF应用层通知NFV orchestrator部署正确的NFs在网络中。 

> *As the data plane becomes more complex, we believe a tension will arise between making these decisions locally on the host versus in the control plane. While network-wide view is important in many cases, to fully exploit the agility beneﬁts of virtual machine-based network functions, a local controller is needed to quickly incorporate state only available at the host (e.g., packet histories, VM-load levels, etc).*

## 实现细节 

- NetVM Core Engine（the DPDK application running in the hypervisor） 

- A NetVM manager 

- 模拟PCI设备驱动器 

- 带修改KVM CPU分配的策略 

- NetLib （用于在VM用户空间创建in-network功能的库） 

![](Leopold-Sun.github.io/blob/master/images/NetVM%E2%80%99s%20architecture%20spans%20the%20guest%20and%20host%20systems.png)

Fig. 8. NetVM's architecture spans the guest and host systems; an emulated 
PCI device is used to share memory. 
实现基于QEMU1.5.0 （包含KVM）和DPDK1.4.1。KVM或QEMU允许常规Linux主机运行一个或多个VMs。 

### NetVM Manager 

NetVM Manager在hypervisor中运行，提供QEMU与DPDK间的信道，QEMU可以将VMs创建、解构的信息通过信道传递给NetVM Core Engine也就是DPDK应用。NetVM Manager启动时，创建一个server socket以与QEMU通信，当QEMU创建新的VM时，其连接socket通知DPDK初始化新VM的数据结构和共享内存区域。连接的形式是：“-chardev socket，path=<path>，id=<id>”。 

NetVM Manager保存关于VMs Trust group的信息、switching 方式。其处理SDN controller与NFV管理器之间的通信。 

![Net VM platforms are distributed in the network (and/or data centers).](Leopold-Sun.github.io/blob/master/images/NetVM%20platforms%20are%20distributed%20in%20the%20network.png)
 

### NetVM Core Engine 

是在hypervisor中运行的DPDK用户应用。NetVM Core初始化的信息包含用户设定（处理器核映射关系、NIC接口设置、并发队列配置）。之后NetVM Core配置大内存页区域、初始化NIC（poll mode driver）以使得即使DMA数据到内存。 

**NetVM有两个设定**

- 收到包并传递给VMs（可配置的分配策略、零复制） 

- 与NetVM Manager通信以同步新VMs的信息 

**Main Look**

> *The main control loop ﬁrst polls the NIC and DMAs packets to huge pages in a burst (batch), then for each packet, NetVM decides which VM to notify. Instead of copying a packet, NetVM creates a tiny packet descriptor that contains the huge page address, and puts that into the private shared ring buffer (shared between the VM and NetVM Core).TheactualpacketdataisaccessibletotheVMviashared memory, accessible over the emulated PCI device described below.*

### 模拟PCI 

VMs通过PCI模拟设备将设备内存映射到自身的地址空间。因为PCI是软件实现的，所以设备内存可以重定向到hypervisor的任何内存位置。 

NetVM需要的两个隔离的内存区域： 

- 私有共享内存（其地址存储在BAR#0寄存器）。用作ring buffers。每个VM都有自己的private shared memory。 

- 大内存页共享内存（地址存在BAR#0寄存器）。在hypervisor中是不连续的，但是可以使用模拟PCI在VM中映射为连续的内存区域。 

![](Leopold-Sun.github.io/blob/master/images/The%20huge%20pages%20spread%20across%20the%20host%E2%80%99s%20memory.png)

Fig. 7. The huge pages spread across the host's memory must be contiguously 
aligned within VMS. Net VM must be able to quickly translate the address of a 
packet from the host's virtual address space to an offset within the VM's address 
space. 
Guest VM会希望使用NetVM的高速IO，运行一个前端驱动器（front-end driver）以使用Linux Userspace I/O framework（UIO）访问模拟PCI设备。该驱动器将PCI中的两个主要的内存区域映射到Guest的内存中，支持应用直接操作入流数据。 

### NetLib and User Applications 

NetLib提供PCI与用户应用之间的接口。用户应用只需要提供一个包含配置设置（如内核数、回调函数（类似Linun内核的NetFilter）等）的结构。当NIC poll到数据包时，调用回调函数，然后应用决定读写数据操作（舍弃、NIC发送、传递给其他VM）。 

![](Leopold-Sun.github.io/blob/master/images/NetLib%20provides%20a%20bridge%20between%20PCI%20device%20and%20user%20applications.png)

NetLib provides a bridge between PCI device and user applications. 
图9表示一个数据流。hypervisor收到一个包；NetLib的一个线程fetch包给用户应用并回调该应用；应用处理包，然会一个action（丢弃……等）；NetLib将该action放入描述符并放到发送队列。 

NetLib提供的也是无锁模型，所以不支持线程间数据交换。 


### NF部署 

NetVM API使用安全通道提供简单的信息交换，类似SDN中的OpenFlow协议。 

NetVM支持的三种功能部署方式： 

- VM provisioning
一个新的VM使用镜像（包含需要的NF代码）启动，启动后，其自动实例化用户代码，然后NetLib公布于VM Manager之间的连接。 
- Application provisioning
假定有一个包括一个空转VMs的pool，NetVM Manager只需要初始化这些VM的用户层进程并暴露必要的信道。 
- 资源池迁移
可能会维持一个NetVM例程池运行在另一个主机上，可以使用KVM迁移工具将VM迁移，迁移后，VM连接至hypervisor的信道，然后开始处理数据。 

## 实验评估

![](Leopold-Sun.github.io/blob/master/images/Forwarding%20rate%20as%20a%20function%20of%20input%20rate%20for%20NetVM.png)

![](Leopold-Sun.github.io/blob/master/images/NetVM%20provides%20a%20line-rate%20speed%20regardless%20of%20packet%20sizes.png)

![](Leopold-Sun.github.io/blob/master/images/NetVM%20scales%20to%2036%20Gbps%20when%20using%20four%20ports.png)

![](Leopold-Sun.github.io/blob/master/images/Inter-VM%20communication%20using%20NetVM%20can%20achieve%20a%20line-rate%20speed.png)

![](Leopold-Sun.github.io/blob/master/images/Average%20roundtrip%20latency%20for%20L3%20forwarding.png)

![](Leopold-Sun.github.io/blob/master/images/State-dependent%20(or%20data-dependent)%20load-balancing%20enables%20flexible.png)

![](Leopold-Sun.github.io/blob/master/images/Time%20to%20NF%20deployment.png)

![](Leopold-Sun.github.io/blob/master/images/Denial%20of%20service%20attack%20mitigation.png)
