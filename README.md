![Geekbrains Logo](https://github.com/ilyastartsdata/introductiontopython/blob/master/gb.png)

# Linux. Workstation.

Course from Geekbrains University

> About Geekbrains: We teach people to master programming, web design and marketing from the ground up. We conduct online courses with internships and free master classes, develop the community, cooperate with employment companies and continuously test new methods to improve the effectiveness of learning.

## List of topics

1. Introduction. Installing the OS
2. Setting up and getting to know the command line interface
3. Users. Management of Users and Groups
4. OS uploads and processes
5. Linux file system arrangement. The concept of a file and directory
6. Introduction to bash scripts. Task schedulers **crontab** and **at**
7. Management of packages and repositories. Network security basics
8. Introduction to docker

## Installing / Getting started

In order to get started, we need the following tools:
  
  1. Ready virtual machine with the following parameters: CPU - 1 Gb, RAM - 2 Gb, hard disk - at least 10 Gb
  2. Downloaded version of the latest Ubuntu Server LTS image
  
Connect the image to the virtual machine as shown in the instructions and launch it.

After the start we will see the first difference between the server version of Ubuntu and the desktop version - the lack of a graphical interface for the installer. The control is simple: Up, Down, Left, Right keys - move through the menu, Space key - select the item, Enter applies the selected item.

![Screenshot_1](https://github.com/ilyastartsdata/linux_workstation/blob/main/screenshots/%231.png)

The next step is to choose the layout. Here we leave everything as default, select the "Done" key and proceed to the network setting point. Select the network interface and press Enter. We are offered several ways to assign an IP address to our server:

  1. Using the DHCP (Automatic DHCP) protocol - this option is most convenient, particularly if your virtual machine has a NAT network connection type
  2. Manual - used when we know the network settings for our server and this option will be used later in the installation review process
  3. Disabled - an option when the network is not used
  
![Screenshot_2](https://github.com/ilyastartsdata/linux_workstation/blob/main/screenshots/%232.png)

Choose the Manual option, press Enter and enter a subnet in x.x.x.x/mask format (192.168.0.0/24), IP address, default gateway and DNS server (use Google DNS 8.8.8.8). Click on "Save". We proceed to the next step.

![Screenshot_3](https://github.com/ilyastartsdata/linux_workstation/blob/main/screenshots/%233.png)

### Disk layout. Completion of OS installation

Before partitioning the disk, let's take a look at the file system structure in Linux. The Linux file system, unlike Windows, has a tree structure. There is a root partition (/ -root) that contains files and directories, which in turn can be mount points for other partitions.

Two partitions are often sufficient for a system to boot up: the root "/" partition and the SWAP partition.

**SWAP** is one of the virtual memory mechanisms in which in which individual memory fragments (usually inactive) are moved from RAM to secondary storage (a separate partition or file), freeing RAM for loading other active memory fragments. If we are deploying a server under Kubernetes, the SWAP partition is not needed.

If there are no special requirements, you can entrust disk partitioning to the installer, but in practice there are often cases where the partitioning offered by the installer does not meet the requirements for the server, e.g. the user's home directory must have a different type of file system. The second difference between the server version of Ubuntu and the desktop version is that the disks are partitioned manually.

In the Filesystem setup menu, select the disk and press Enter, and the partitioning menu will appear:

![Screenshot_4](https://github.com/ilyastartsdata/linux_workstation/blob/main/screenshots/%234.png)

Select Add Partition, press Enter and fill in the fields as follows:

  1. Size (size in GB) - 1G - volume of space per section.
  2. Format - ext4 - type of file system in which the partition will be formatted.
  3. Mount - /boot - mount point. The /boot partition contains the boot files needed to start the system (bootloader, kernel, etc.). It is recommended to put it      on a separate partition, in particular if we use LVM for partitioning disks, as neither BIOS nor EFI are able to work with LVM partitions.

Before continuing the breakdown, we will define the concept of LVM.

**Logical Volume Manager (LVM)** is a data volume management system for Linux. It makes it possible to create logical volumes on top of physical partitions or even unmarked hard disks, which in the system itself will be visible as ordinary block devices with data, i.e. as ordinary partitions. We recommend using it if you install the Ubuntu server version, as it allows you to work more flexibly with existing disk space: increase or decrease the partition without losing data and with minimal system downtime.

The remaining partitions can be partitioned using LVM. To do this, select the free space, press Enter. In Size, we either specify nothing (the installer uses all available space) or specify the size. Choose Leave unformatted and press Create.

![Screenshot_5](https://github.com/ilyastartsdata/linux_workstation/blob/main/screenshots/%235.png)

Next step: we create the Volume Group. Select Create volume group, press Enter, leave the group name by default and press Create. Volume Group in terms of LVM is an abstraction that combines physical volumes (real hard drives). The result is a single disk space that we can already partition at our discretion. In the partitioning menu, we select the group name and press Enter. This opens a menu in which we are interested in the Create Logical Volume item.

![Screenshot_6](https://github.com/ilyastartsdata/linux_workstation/blob/main/screenshots/%236.png)

We create the first Logical Volume. From an operating system point of view, this is an ordinary section. This section in our case is SWAP. There are several approaches to choosing the SWAP section:

  1. Most common: SWAP equals the amount of RAM.
  2. The second approach is based on Oracle's recommendation: if the system has up to 16 GiB of RAM, the SWAP size is equal to the amount of RAM, and if it is above 16 GiB, the SWAP size is equal to 16 GiB.
  3. And the third approach: if we are preparing the system for the Kubernetes cluster, the SWAP section is not needed at all.
  
In our case, we set the partition size equal to the amount of RAM allocated to the virtual machine:

![Screenshot_7](https://github.com/ilyastartsdata/linux_workstation/blob/main/screenshots/%237.png)

And we complete the layout of the disc with a section "/", under which we give all the remaining space. To do this, we repeat the same steps as for the SWAP partition.

We finish the partitioning by clicking "Done" and then select "Continue":

![Screenshot_8](https://github.com/ilyastartsdata/linux_workstation/blob/main/screenshots/%238.png)

After that, we proceed to the completion of the installation. We create a user who will be the main user of the system or the administrator of the computer, set a password and set a server name:

![Screenshot_9](https://github.com/ilyastartsdata/linux_workstation/blob/main/screenshots/%239.png)

The next step is to set up SSH. In our case, we activate only the Install SSH server point:

![Screenshot_10](https://github.com/ilyastartsdata/linux_workstation/blob/main/screenshots/%2310.png)

Next, we are asked to install additional features for the server, we skip this point, click on "Done" and wait for the OS to be installed.

![Screenshot_11](https://github.com/ilyastartsdata/linux_workstation/blob/main/screenshots/%2311.png)

### Sudo utility, administrative actions in the OS

After installing the OS, we need to perform a number of actions related to the configuration and subsequent administration of the system. In Linux, all actions related to administration as well as critical changes to the operating system are given to a special user - root. Any action taken on behalf of this user is irreversible and in the event of inaccurate or improper use may render the system inoperable. To avoid possible destructive actions or hacking into the system, working on behalf of user root is prohibited in many distributions. The sudo utility exists to perform administrative actions. 

**Sudo** is a utility that provides root privileges to perform administrative operations according to your settings. It allows controlling access to important applications in the system.

For further actions related to system administration, we will use this utility. 

#### Setting up the network using ip and nmcli utilities

The first thing we will look at is how to set up the network. In Ubuntu Server 18.04 and above, Netplan is responsible for network configuration. At the same time, we can change the network configuration in the classical way, using command-line tools: ip from the iproute2 package, nmcli (in the case of Ubuntu desktop version) or ifconfig from the net-tools package.

**Important:**

  1. Changing network settings using command line utilities usually "lives" until the first reboot of the system. 
  2. In Ubuntu Desktop, network configuration is the responsibility of NetworkManager (nmcli command line management utility) or the ifupdown script (configuration file /etc/network/interfaces and networking service). Netplan allows all these methods to be structured.
  3. Changing network settings is an action that requires superuser rights. It is customary to use sudo when performing actions related to configuration changes.
  
Let's take a look at the utilities in order. All the utilities under consideration are console applications, i.e. a command line is required to work with them. In Linux, the standard command line application is the terminal, and we will look at it in more detail in the next lessons. The following format is used in work with console applications: command parameters_commands, and symbols -, -- . You can get help on the syntax of many commands and possible parameters using the following record format: ```--help``` command.

#### Change or set up the network using the ip utility

  1. The **ip** utility is a console application that can be used to obtain information about the configuration of network interfaces, configure network interfaces and change the routing table.
  2. Viewing available network interfaces and current settings (does not require superuser rights): ```ip a```.
  3. Change or assignment of an IP address to the network interface:
  
  ```shell
  sudo ip addr add {id_addr/mask} dev {interface}
  ```
  
  Example:
  
  ```shell
  sudo ip addr add 192.168.0.110/24 dev enp0s25
  ```

  The name of the interface can be found in the ```ip a``` output. It is necessary for the operating system to be able to match a particular physical device to the software properties assigned to that device (IP address).
  
  4. Setting the default route:
  
  ```shell
  sudo ip route add default via {ip_gateway}
  ```
  
  Example:
  
  ```shell
  sudo ip route add default via 192.168.0.1
  ```
  
#### Changing or configuring the network using the ifconfig and route utilities from the net-tools package

  1. View available interfaces and current settings (does not require superuser rights): ```ifconfig -a```.
  2. Change or assignment of an IP address to an interface:
  
  ```shell
  sudo ifconfig {interface} {ip_addr} netmask {netmask}
  ```
  
  Example:
  
  ```shell
  sudo ifconfig eth0 192.168.0.100 netmask 255.255.255.0
  ```
  
  3. Setting the default route:
  
  ```shell
  sudo route add default gw {ip_gateway}
  ```
  
  Example:
  
  ```shell
  sudo route add default gw 192.168.0.1
  ```
  
#### Changing or configuring the network using the nmcli utility

**Nmcli** is a utility for managing the NetworkManager service from the command line. It can be used when working with the desktop version of Ubuntu.

  1. View available interfaces and current settings (does not require superuser rights): ```nmcli device``` - will show existing devices, ```nmcli -p device show``` - will show network settings on all available interfaces.
  2. Setting up the network connection using nmcli (add static address):
  
  ```shell
  sudo nmcli connection add con-name "{name_connection}" ifname {device} autoconnect {yes/no} type ethernet ip4 {ip_addr} gw4 {ip_gateway}
  ```
  
  ```{Name_connection}``` - name of connection (it is better to use one word in English), ```{device}``` - network interface, ```autoconnect {yes/no}``` - allow connection at system start automatically or not, ```{ip_addr}``` - IP address, ```{ip_gateway}``` - ip gateway. 

  Example:
  
  ```shell
  sudo nmcli connection add con-name "connection1" ifname enp0s2 autoconnect yes type ethernet ip4 192.168.0.100 gw4 192.168.0.1
  ```

#### System update. Installation of VM add-ons if necessary

The next step to be taken after installing the operating system is to install add-ons for the virtual machine and update the operating system. The add-ons are particularly necessary if we are using a virtual machine running VirtualBox.

##### Installation of guest OS extensions (VirtualBox)

The guest operating system add-ons contain a set of applications and drivers required to optimise, improve performance and ease of use.

  1. In the Virtual Machine Management menu, select 'Devices' and at the end of the list click 'Connect guest OS add-on image'.
  2. The installation of applications requires superuser rights, so we will perform all subsequent actions using the sudo utility. In the installed OS, we execute the following command: ```sudo apt update```. Apt is a utility for managing packages, in other words, applications in Ubuntu. The ```update``` option tells the utility that it needs to update information about available packages in repositories (repository - repository of packages).
  3. Using the apt utility, we will download and install the following packages: ```sudo apt install perl make gcc -y```. Here the ```install``` parameter tells the ```apt``` utility that three packages need to be installed and the ```-y``` parameter means that ```apt``` will install the packages without user confirmation.
  4. Ubuntu Server does not include the option to auto-Mount (connect) external media. The next step is to mount a connected OS add-on image (requires superuser rights): ```sudo mount/dev/cdrom/mnt``` , where ```mount``` is a utility for mounting disk devices in Linux, ```/dev/cdrom``` is a virtual CD reader. It can also be real, physical. The directory ```/mnt``` is a ```cdrom``` mount point.
  5. Go to the /mnt directory: cd /mnt. C" (Change Directory) - a command to move between directories in Linux. We run the add-on installation script: ```sudo ./VBoxLinuxAdditions.run```.
  
Once the add-ons have been installed, the operating system must be rebooted, but before this can be done, the updates must be installed. To do this, we will execute the command ```sudo apt upgrade -y```. The ```upgrade -y``` parameters tell the apt utility that it is necessary to upgrade the system without asking the user for confirmation. When the upgrade is complete, we reboot the operating system: ```sudo reboot```.

##### Installation of OpenSSH Server and Midnight Commander file manager

Ubuntu Server OpenSSH Server is installed automatically; if you use Ubuntu Desktop, you must install it yourself: ```sudo apt install openssh-server -y```. It is also recommended to install the Midnight Commander: ```sudo apt install mc -y``` file manager for easy navigation and work with files.

## Contributing

Pull requests are welcome.

## Source

[Geekbrains](https://geekbrains.ru)
