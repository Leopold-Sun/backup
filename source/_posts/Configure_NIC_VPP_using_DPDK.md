---
title: Configure ethernet to vpp instance using dpdk
date: 2018-12-10 17:08
tags: [VPP]
---

### Preliminaries

- Pull vpp from github
> `git remote add origin https://gerrit.fd.io/r/vpp`
> `git pull origin master`

- Make and install dependencies under *root*
> `make install-dep`
> `make build` or `make build-release` *make build for debugging*
> `make pkg-deb`
> `dpdkg -i build-root/*.deb`

- DPDK installed
> Or you can use dpdk-devbind.py from "build-root/install-vpp_debug-native/external/share/dpdk/usertools"
> `cp dpdk-devbind.py /usr/bin` *use this instruction to make it easy-used*

- Grub information
> `sudo vim /etc/default/grub` to configure *GRUB_CMDLINE_LINUX_DEFAULT="quiet splash default_hugepagesz=1G hugepagesz=1G hugepages=4 iommu=pt intel_iommu=on"* 
> `sudo update-grub`
> `reboot`
> reboot your machine to make the grub configuration valid
> `cat /proc/cmdline` to check again

### Check ethernet stats and bind it to dpdk

- The NIC you want to bind should not be **active**
> `dpdk-devbind.py -s` *we could use this to gain pci address of the nic and its driver information*
> If the nic is active, we could disconnect it physically or use `ifconfig ${nic name} down` to shut it down

- Use vfio-pci driver module
> `sudo modprobe vfio-pci` *use vfio-pci module*
> `lsmod | grep vfio` *for check use*
> `sudo dpdk-devbind.py -b vfio-pci ${nic name or its pci number}` *bind nic to dpdk*

- If the bind option goes wrong
> `dmesg | grep -i vfio` *to check error information*
> I have encounterred one binding error withh error-22 from dmesg and it turned out my NIC was not supported by dpdk or vfio-pci module
> If binding option succeeds, we could use `dpdk-devbind.py -s`  to check

### Using dpdk nic through vpp instance

- Before creat one new instance
> `service vpp stop` *to clean legacy of the last vpp instance*
> or remove all files in "/dev/shm"

- Modify vpp startup.conf
> Using dpdk needs to be claimed in "/etc/vpp/startup.conf"
> `dpdk {`
> `dev ${pci number of dpdk nic}`
> `}`

- Enter into vpp interface
> `vpp -c /etc/vpp/startup.conf`

- Set the nic up
> `show interfaces` *It turns out the nic has been down*
> `set int state ${nic name} up`

- Assign ip address to the nic
> `set int ip address ${nic name} ${ip address}` *I was told that modifying the first 16 bits of IPv4 number will make it not belongs to the same internet*
