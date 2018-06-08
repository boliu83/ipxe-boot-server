# Build your own PXE boot server

This article is a step by step guide for building your own PXE boot infrastructure which can be used to boot both legacy BIOS and EFI based hardware from network. There are many articles on the Internet for building PXE boot infrastructure however I found most of them does not work for EFI based hardware. I use [iPXE](http://ipxe.org/) as the boot image and [dnsmasq](http://www.thekelleys.org.uk/dnsmasq/doc.html) as I found it's dead simple to setup those two software. 

## Lab setup

1. PXE Boot server (**Centos 7.5**) with two interfaces. Interface 'eth0' is for sysadmin to login and manage the server. Interface 'eth1' is connected to the PXE boot subnet.
2. PXE Boot subnet (10.0.0.0/24). This is an isolated network created just for booting new servers on the network. We suggest to use isolated network for PXE boot to prevent it from interfering with existing DHCP servers on the network. 
3. PXE Boot client. A VMware VM set to PXE boot from network.  

![](images/lab-diagram.png)

## Step by step guide

1. Make network interface connected to PXE boot network is correctly configured

   ```bash
   [sysadmin@pxeboot-server ~]$ sudo ifconfig eth1 10.0.0.1 netmask 255.255.255.0 up
   ```

2. Update PXE boot server to latest version

   ```bash
   [sysadmin@pxeboot-server ~]$ sudo yum update -y
   ```

3. Install required packages

   ```bash
   [sysadmin@pxeboot-server ~]$ yum install -y ipxe-bootimgs dnsmasq 
   ```

4. Create TFTP root directory

   ```bash
   [sysadmin@pxeboot-server ~]$ mkdir /tftpboot
   ```

5. Copy iPXE boot images to TFTP directory

   ```bash
   [sysadmin@pxeboot-server ~]$ sudo cp /usr/share/ipxe/{undionly.kpxe,ipxe.efi} /tftpboot/
   ```

   `undionly.kpxe` is for legacy BIOS based hardware
   `ipxe.efi` is for EFI based hardware

6. 

