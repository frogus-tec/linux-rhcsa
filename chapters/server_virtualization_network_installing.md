# Server Virtualization and Network Installing

Server virtualization allows a physical computer to host several virtual machines, each of which acts as a standalone computer running an OS.

Virtualization software deployed directly on bare-metal host machine is referred to as the hypervisor software. KVM (Kernel-based Virtual Machine) is part of the Linux Kernel and comes as a native hypervisor with RHEL7. QEMU then uses the physical-to-virtual CPU mappings provided by KVM. libvirt is the virtualization management library.

```
# check if the processor supports virtualization
lscpu | grep -i virtualization
--> Virtualization:        VT-x
--> Virtualization type:   full
# check for flag vmx (intel processor) 
grep vmx /proc/cpuinfo
```

```
# install virtualization packages
yum install qemu-kvm qemu-img libvirt libvirt-client
# rather than memorising these packages see groups
yum grouplist hidden
yum groupinstall "Virtualization Client"
yum groupinstall "Virtualization Tools"
yum groupinstall "Virtualization Platform"
```

#### Virtual Network Switch

A virtual network switch is a libvirt-constructed software switch to allow the virtual machines to be able to communicate with the host and among one another. The host and virtual machines see this switch as a virtual network interface. 

```
systemctl status libvirtd
# see configuration
ip addr show vibr0
```

The default mode of operation for vibr0 is NAT with IP masquerading. NAT allows the network traffic of guest operating system to access external networks via the IP address of the host machine. 


#### Virtualization Management Tools

* The default hypervisor and virtual machine management software is libvirt. The Virtual Machine Manager is the graphical equivalent of virt-install and virsh. 

```
# open virt-manager graphical tool
virt-manager
```

#### Define a NAT Virtual Network Using virsh
```
yum install libvirt
# create rhnet_virsh.xml definition in /root
virsh net-define /root/rhnet_virsh.xml
# set automatic start up
virsh net-autostart rhnet_virsh
# start new virtual network
virsh net-start rhnet_virsh
# list all virtual networks
virsh net-list
# see details of virtual network
virsh net-info rhnet_virsh
```

#### Configure FTP Installation Server

FTP is a standard networking protocol for transferring file between systems. In RHEL there is a version called very secure FTP or vsFTP which allows us to enable, disable and set security contorls on incoming service requests. The vsFTP daemon `vsftpd` communicates on port 21. 

```
yum - y install vsftpd
```

#### The wget Utility 

`wget` is a non-interactive file download utility that allows you to retrieve a single file or entire directory from FTP, HTTP(S).

```
wget -d www.redhat.com
```

#### Maintaining and Managing Log Files

A script called `logrotate` in `/etc/logrotate.d` manages the rotation of logfiles by source `/etc/logrotate.conf`

#### The Journal 

```
# see verbose output
journalctl -o verbose
```