---
layout: post
title: install xen-4.8 on debian9
---

Most of us may have used virtualbox or vmware, they have friendly UI and are easily to use, they run under a host OS, just like a normal software. but xen is different, xen runs directly on hardware, it is loaded by the bootloader before OS, then it loads the first special VM(dom0), dom0 has access to hardware, xen-tools are installed on dom0 used to communicate with xen hypervisor and manage other VMs, other VMs can't access the hardware directly. in this article, i will tell you how to install xen-4.8 on debian9 64bit, and install a windows guest OS.

**Content:**  

--------------------------------------------------------------

[1. Host OS preparation](#1)  
&nbsp;&nbsp;&nbsp;&nbsp;[1.1 Install xen hypervisor](#1.1)  
&nbsp;&nbsp;&nbsp;&nbsp;[1.2 Install xen tools](#1.2)  
&nbsp;&nbsp;&nbsp;&nbsp;[1.3 Configuration network for guest OS](#1.3)  
[2. Guest OS installation](#2)    
[3. Optimize](#3)  

---------------------------------------------------------------

<h2 id="1">1. Host OS preparation</h2>
<h6 id="1.1">1.1 Install xen hypervisor</h6> 

to install xen hypervisor, all you need to do is:  
```
&nbsp;&nbsp;&nbsp;&nbsp;apt-get install xen-linux-system-amd64
```
after you install the xen hypervisor, the grub will automatically set xen hypervisor as default boot item. now you can reboot.  

<h6 id="1.2">1.2 Install xen tools</h6>

xen has many toolstack which can be used to manage VMs. xl is the default tools. to install xl-tools. type:  
```
apt-get install xen-tools
```
if you have already reboot in the previous step. now you can type:  
```
xl top
```
it will show the dom0 status.  

<h6 id="1.3">1.3 Configuration network for guest OS</h6>

now we have installed xen hypervisor and xen-tools. the next step is to configure the network for guest OS. unlike virtualbox or vmware, they can create virtual ethernet automatically for us, we need create it manually.
first we need to install bridge utilities.  
```
apt-get install bridge-utils
```
now we can specify network information through */etc/network/interfaces* file.  
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet manual

auto xenbr0
iface xenbr0 inet dhcp
bridge_ports eth0
```
above we create a virtual ethernet *xenbr0* and bridge it to eth0.
now restart networking:  
```
ifdown -a && ifup -a
```
check to make sure it worked:  
```
brctl show
```

<h2 id="2">2. Guest OS installnation</h2>

to install a xen guest, we need create a config file that can describe the hardware information about the guest OS. a sample *windows_7.cfg*:    
```
builder= 'hvm'
memory = 2048
vcpus=4

name = "windows_7"
vif = [ 'bridge=xenbr0' ]
acpi=1
apic=1
disk = [ 'file:/home/windows_7/windows_7.img,hda,w', 'file:/home/windows_7/windows_7.iso,hdc:cdrom,r' ]

device_model_version = 'qemu-xen'
xen_platform_pci=1

boot="dc"
sdl=0
vnc=1
vnclisten=""
vncpasswd=""
serial='pty'
```
pay special attention to the *disk* line, we have two disks, the first one is a .img file used for windows system intallation. you need to create it manually first, size more than 20GB is recommended, the second one is a ISO file, that is the windows installation ISO image which you can download from internet.  
now finnaly we can start the VM:  
```
xl create windows_7.cfg
```
after that, we can use vncviewer to connect to the guest OS and complete the OS installation.  
```
gvncviewer <dom0-ip-address>
```
now you can enjoy it, but i recommend you continue to read the next section.

<h2 id="3">3. Optimize</h2>

actually xen uses intel VT-x and AMD-V to boost its cpu and memory virtualization. for device IO, it uses qemu to emulate it, and it is very slow because it's emulated. to gain good performace, we can use PV drivers. after install the windows guest, we can download PV drivers [here](https://xenproject.org/developers/teams/windows-pv-drivers.html) and install it in the guest OS.

*References:*  

[https://wiki.xenproject.org/wiki/Xen_Project_Beginners_Guide](https://wiki.xenproject.org/wiki/Xen_Project_Beginners_Guide)
