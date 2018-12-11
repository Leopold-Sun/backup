---
title: How to know that your nic is supported by dpdk or not
date: 2018-12-11 16:55
tags: [VPP]
---

- Preliminary

> If you are not sure whether the nic is supported by dpdk, just assume it is and use normal instructions to bind it to dpdk.

- Try to bind

> `dpdk-devbind.py -s`
> If have decided which one driver is supposed to be used (uio-pci-generic , igb-uio or vfio-pci), just disconnect the nic physically and bind it
> `sudo dpdk-dvebind.py -b ${driver name} ${nic pci number}`
> If this instruction goes wrong, perhaps the needed module is not be inserted or moduled
> `modprobe vfio-pci`
> `insmod ${path}/igb_uio.ko`

- Configure vpp startup.conf which resides in /etc/vpp

> `sudo vim /etc/vpp/startup.conf`
> Modify contents of startup.conf
> `uio-driver ${driver name and default igb-uio}` *It turns out using vfio-pci needs not to be declared in this instruction*
> `dev ${nic pci number}`

- Start vpp

> `sudo vpp -c /etc/vpp/startup.conf` *this instruction could normally start the vpp without debug*
> `systemctl start vpp.service` *start vpp.service with systemctl*
> `systemctl stop vpp.service`
> `systemctl status vpp.service`
> If the binded nic is not supported by dpdk, the status contains the following information but with slight differences
> *Dec 11 16:49:42 pold vpp[890]: dpdk: Unsupported PCI device 0x1969:0x10a0 found at PCI address 0000:02:00.0*
