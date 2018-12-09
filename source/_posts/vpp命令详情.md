---
title: VPP相关命令
date: 2018-12-09
tags: [VPP,NIC,DPDK,VFIO-PCI]
categories: VPP userspace stack
---

##### _非交互模式（Linux下）_

Linux创建veth接口对： ip link add name vpp1out type veth peer name vpp1host 

Linux启动veth接口的两端：ip link set  ${veth-name} up

Linux为veth配置IP地址： ip addr add 10.10.1.1/24 dev vpp1host  

```
ip link add veth0 type veth peer name veth1
ip addr add 10.1.0.1/24 dev veth0
ip addr add 10.1.0.2/24 dev veth1
```

Linux查看接口信息：ip addr show <NIC name>

Linux创建vpp实例：sudo vpp unix {nodaemon cli-listen /run/vpp/cli-${name}.sock} api-segment { prefix v${name} }

Linux 用vppctl发送命令给vpp：sudo vppctl -s /run/vpp/cli-${name}.sock ${cmd}

两个ip地址ping（前ip ping 后ip）：ping #ip1-I #ip2   //其中“-I”为大写的i

##### _交互模式（vpp下）_

VPP启动vpp实例：sudo vppctl -s /run/vpp/cli-${name}.sock

VPP产看版本号：show version

创建host interface： creat host-interface name ${name}

turn up host-interface:  set int state ${name} up

创建interface： creat interface ${name}

启动interface： set int state ${name} up 

assigning ip address to interface： set int ip address ${name} ${ip addess} 

查看接口ip：show interfaces address

查看接口统计：show int

VPP设置接口rx、tx队列大小：set dpdk interface descriptors interface0/0/0  rx 1 tx 1024

跟踪 dpdk 接口数据包 (dpdk-input 是节点的名字, 想跟踪任何节点都可以)：trace add dpdk-input 8 

跟踪 vhost 接口数据包：trace add vhost-user-input 8

跟踪 veth 接口数据包：trace add af-packet-input 8 

查看bridge接口情况：show bridge-domain

查看node逻辑图：mac show vlib graph

查看vpp线程：show threads

查看二层转发流表：show l2fib

查看路由表：show ip fib 

查看arp：show ip arp

查看主线程cpu亲和性：show affinity





#### **vpp command links**

> vpp命令总结：https://wanwang.aliyun.com/info/1580284.html
>
> vpp安装运行：https://wanwang.aliyun.com/info/1580286.html?spm=5176.100101.0.0.5b3b5bf7mY16H8

**Linux-Networking Conmmand Links**

> Linux virtual interfaces:https://gabhijit.github.io/linux-virtual-interfaces.html
