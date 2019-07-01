# Hypervisor Setup

Proxmox VE is a complete open-source platform for enterprise virtualization. With the built-in web interface you can easily manage VMs and containers, software-defined storage and networking, high-availability clustering, and multiple out-of-the-box tools on a single solution.

## Steps

- Installing Proxmox Hypervisor on the server
- Setting up VM on Hypervisor

## Installing Proxmox VE on the server

#### Refer (https://pve.proxmox.com/wiki/Installation)

1. Download Proxmox VE ISO from https://www.proxmox.com/en/downloads/item/proxmox-ve-5-4-iso-installer
2. Create bootable USB using the ISO
3. On the server, change the boot order for Flash Drive through iDRAC settings
4. After the ISO is booted up, install the Hypervisor
5. After installation, you can access web console using ip displayed on the screen

## Installing VM on Hypervisor

1. Login via web console using the credentials
2. Select Datacenter->node->local(node)->Content and upload the ISOs required by VMs
3. Now, Create a new VM by choosing "Create VM" on right top
4. Give a name to VM
5. Select the ISO from local storage
6. Next, keep defaults under System
7. Then, choose Hard disk Bus Device as VirtIO Block and cache as Write-back.
8. After that, you need to configure CPU
9. Later, in the next step, you need to allocate memory
10. Then, you need to configure your network interface
11. Check all the details and click on the Finish button
12. Now VM can be accessed via noVNC console
13. After installation of OS and during first boot, Type command "dhclient" to configure the network for the device

    ```
    sudo dhclient
    ```

14. Now, the VM is good to go
