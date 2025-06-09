# Setup Labs - Laptop or Cloud

### Overview
- VirtualBox
  - Deploying VMs
  - Multiple VMs
  - Networking and Troubleshooting Network
  - Snapshots and Restore VMs
- Vagrant Basics

### Lab setup on Labtop using Virtual Machine
### Virtualization S/W
- or Hypervisor, these are known as Type 1 hyper-v example are vmware ESXi and Microsoft Hyper-v. They're installed dicrectly on bare metal such as a laptop or server and then the VM's are created on that. These are use in enterprises and production environment where yoe need to create and manage large number of VM's and such these hyper-v have high rresource requirements they also need to be installed and configured directly on the laptop and they are expensive as well. A lot of people do use this home lab expecially is they have a systems with high resources but that's not what we want here. For our purpose there are other solutions available that better suits our needs and are easy to manage. And those are type 2 visorhypervisor, they're are hypervisors that runs on an existing operating system such as oracle virtual box, and VM workstation these allow you to easily get started with virtual machines on our laptop without having to install any other operating system or reimage our laptop. 

So going forward when we say host operating system, we are referring to main OS on our laptop and when we are talking about guest OS we are referring to the VM's that ar created on the hypervisors on the operating system.

### Oracle VirtualBox
- Free
- OpenSource
- Install Anywhere - Windows, Linux, MAC
- Snapshots & Clones
- Run Multiple VMs
- Networking

### VMWare Workstation
- Commercial
- Install Anywhere - Windows, Linux
- Snapshots & Clones
- VMWare Player
  - Free
  - Windows/Linux
  - One VM at a time
  - No Snapshots
- VMWare Fusion
  - Free
  - Mac
  - One VM at a time
  - No snapshots

### Install Oracle VirtualBox
```
https://www.vitualbox.org/wiki/Downloads
```

### Images
```
https://www.osboxes.org/centos/
```

### IP Address
```
guest> ip addr show
```
```
ip addr add 192.168.1.10/24 dev eth0
```

### Start SSH Services
```
systemctl status sshd
```
### Connecting to VM on Mac and Windows
MAC
```
port forwarding
ip addr show
ssh root@127.0.0.1 -p 2222
```
Windows
```
bridge adapter
ifconfig
```
### Networking
- Networking in VirtualBox
- Networking Adapters
- NAT
- Bridge
- Host Only
- Internet Connectivity

### IP Address

### Internet Connectivity
- Network Address Translation (NAT) Network - Possible
- Host-Only Network - Possible with IP Forwarding 
- Bridged Network - Possible

# VirtualBox Networking
### Objectives
- Networking in VirtualBox
- Networking Adapters
- NAT
- Bridge
- Host Only
- Internet Connectivity

### IP Address
Our computer system like our laptop and servers have different kind of interfaces or adapters that are use for connectivity such as `wired ethernet interfaces` to connect to a LAN network through a hub or a switch using a cable and `wireless interfaces` to connect to a Wi-Fi. Once they connect to a network they're get an IP address assigned either manually or dynamically if there is a DHCP server in the environment.
- An IP address is assigned to an interface. in this case we have alaptop that is connected using an internet cable to a switch in our home.
```
ip addr show
2: enp0s3: ...
    inet 192.168.1.5/24 ...
```
- And when have another adaptor(eg. a Wi-Fi adaptor) and you attached that to the same network but this time through a Wi-Fi then that interfaces got another IP address assigned.
```ip addr show
2: enp0s3: ...
    inet 192.168.1.5/24 ...
3: wlp0s2: ...
    inet 192.168.1.6/24 ...
```
### Virtual Networking
- Host-Only Network - explain and give example, we create a private network, can reach each other but not in the outside world
- Network Address Translation (NAT) Network - other scenario we have another physical device connected to the same network and it holds some server let's say db and the vm in the first device which in the host-only network wanted to access the db in the other device. In the current setp up the Vms can't be able to do that because they are on the host-only network and they cant reach outside of that host, so for this we introduced a NAT Network, it is similar to the host only network but this can access the outside world and now the VMs can access the db. The way it will do is that it will translate the IP adress from VM's to the Host and then pass to the db and then it process and sends back the data but upon sending it to the VM's the NAT Network intercepts and instead it uses back the IP Adress of the VMs that has been replace earlier. And That's why it called address translation.
- Bridge Network - To access from external to internal VMs it acts as an extension of the LAN network. We dont really need to create a bridge network like what we use to the host-only network and NAT network. The Bridge network is always there so we just have to connect to it. SO once the VM's connect to this network this time they get an IP address in the same IP range as of that external LAN network.

### Internet Connectivity
- NAT Network - Possible  to connect the internet
- Host-Only Network - Possible with IP Forwarding
- Bridge Network - Possible  to connect the internet

### Port Forwarding
- Allows us to map a port on the host to a port on the guest. For example in a scenario, the VMs have a web server and since its a NAT setup external host can reach the VMs, so configuring the port forwarding can help the port forward can help recieve the external host request even though it's a NAT setup because the port forwarding forwards the traffic from the port fon the host to the port on the guest.
```
ssh 192.168.1.10
# another way if you don't know the IP address of the VM
ssh 127.0.0.1 -p 2222
```

![alt text](image.png)

### Vagrant
- Download
- Create VM
- Create Networks
- Configure Networks
- Configure Networking
- Configure Port Forwarding
- Boot up VM

Vagrant helps us automate all of this task and do all of thiswith a single vagrant `up` command that we don't have to search images of the operating systems or download them or create networks or configure a port forwarding manually. Vagrant does all of that automatically it useful when you have a very complex and multiple VMs and if you plan to deploy and manage the entire setup together.

To get started go to the vagrant website
https://www.vagrantup.com/

Once installed
```
vagrant init centos/7
```
https://app.vagrantup.com/boxes

```
ls
Vagrantfile
```

To start the vagrant box
```
vagrant up
```
### Vagrant CommandsS
```
vagrant
```

SSH to vagrant
```
vagrant ssh
```

### Vagrantfile
```
Vagrant.configure("2") do | config|
    config.vm.box = "centos/7"
    config.vm.network "forwarded_port", guest: 80. host: 8080
    config.vm.synced_folder "../data", "/vagrant_data"
    config.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
    end
    config.vm.provision "shell", inline: <<-SHELL
        yum update
        yum install -y httpd
    SHELL
end
```
### Vagrant Providers
- VirtualBox
- VMware
- Hyper-V
- Docker
- Custom