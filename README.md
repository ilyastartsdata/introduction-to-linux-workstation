![Geekbrains Logo](https://github.com/ilyastartsdata/introductiontopython/blob/master/gb.png)

# Linux. Working station.

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

## Contributing

Pull requests are welcome.

## Source

[Geekbrains](https://geekbrains.ru)
