# Hypervisor Setup

Citrix Hypervisor is a comprehensive server virtualization platform with enterprise-class features built in to easily handle different workload types, mixed operating systems and storage or networking configurations (https://docs.citrix.com/en-us/citrix-hypervisor/citrix-hypervisor-8.0.pdf)

## Steps

- Installling Citrix Hypervisor on the server
- Installling Openxenmanager on the system
- Setting up VM on Hypervisor

## Installing Citrix Hypervisor on the server

1. Download Citrix Hypervisor ISO from https://www.citrix.com/downloads/citrix-hypervisor/product-software/hypervisor-80-express-edition.html
2. Create bootable USB using the ISO
3. On the server, change the boot order for Flash Drive through iDRAC settings
4. After the ISO is booted up, install the Hypervisor (refer https://www.youtube.com/watch?v=uuKc0uiQgNs)
5. After installation, you can access xs-console on the Hypervisor

## Installing openxenmanager on PC

OpenXenManager is Open Source Free software and popularly known as alternate of XenCenter for linux. It is mainly written in python and pyGTK helps user to interact on its GUI

#### Make Sure you are installing Xenmanager on Ubuntu 16.04

1. Install xenmanager dependency
   - sudo apt-get install python-gtk2 glade python-gtk-vnc python-glade2 python-configobj
2. Download xen manager using git clone
   - git clone https://github.com/OpenXenManager/openxenmanager.git
3. Install python-setup tool
   - sudo apt-get install python-setuptools
4. Navigate to openxenmanager
   - cd openxenmanager
5. Install xen manager using python
   - python setup.py install
6. Now you can open XenManager
   - ./openxenmanager

#### Refer (https://www.linuxhelp.com/how-to-install-open-xen-manager-on-ubuntu-16-04)

## Installing VM on Hypervisor

#### Before installing VM, go to Network tab and add an External Network with server connected NIC

1. Right click on the server domain on the Resource panel and choose new VM option
2. Select a template
3. Give the new VM a name
4. Specify the operating system installation media (Refer NFS Server Setup)
5. Choose a home server
6. Configure CPU and memory
7. Configure storage
8. Configure networking
9. Complete new VM creation
10. Start the VM
11. After the VM boots up, check out if interface has an IPv4 address
    - If it doesn't, use "ip addr" command to list interfaces
    - To bring the interface up, use command "ifconfig <<interface>interface> up"
    - Type command "dhclient" to configure the network for the device
12. Now, the VM is good to go

## Setting Up NFS Mount on Ubuntu 16.04

Network File System, is a distributed file system protocol that allows you to mount remote directories on your server. This lets you manage storage space in a different location and write to that space from multiple clients.

1. Installing NFS server
   - sudo apt-get install nfs-kernel-server
2. Make a share directory called nfs
   - sudo mkdir /var/nfs/general -p
3. Change the directory ownership
   - sudo chown nobody:nogroup /var/nfs/general
4. Open the /etc/exports file in your text editor
   - sudo nano /etc/exports
5. Type this out

```
   /var/nfs/general ip-of-host(rw,sync,no_subtree_check)
   /home ip-of-host(rw,sync,no_root_squash,no_subtree_check)
```

6. Restart the NFS server
   - sudo systemctl restart nfs-kernel-server
7. Check the firewall is enabled and if so, then permit
   - sudo ufw allow from ip-of-host to any port nfs
8. Now create two directories for our mounts

```
sudo mkdir -p /nfs/general
sudo mkdir -p /nfs/home
```

9. Mount the shares by addressing

```
sudo mount ip-of-host:/var/nfs/general /nfs/general
sudo mount ip-of-host.0:/home /nfs/home
```

- To mount on boot

```
ip-of-host:/var/nfs/general    /nfs/general   nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
ip-of-host:/home       /nfs/home      nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
```

10. Now you can access files on /home directory (Move your ISOs to /home)
