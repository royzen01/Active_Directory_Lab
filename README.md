# Active Directory Lab

## Description

For this lab I created a virtualized Active Direcotory enviorment using VirtualBox. The project consisted of two virutal machines: 

The first virtual machine will be the Domain Controller (DC) and it will host Windows Server 19. It will also have two seperate NICs. One for connecting to the internet and one for internal connections. I also ran a PowerShell script to automate the process of adding 1k+ users to the domain. The services running on the server are:

* <b> AD DS: </b> I installed Active Directory for user management
* <b> NAT: </b> NAT was used for connectivity to the clients as well as providing said clients access to the internet.
* <b> DHCP: </b> A DHCP server will be needed in order to assign IP Addresses to each client.

The second virtual machine would host one test client device with an internally connecting NIC. This client would be used to ensure the server is properly configured.

## Topology

<img src="https://i.imgur.com/w2YHkGD.png" />

## Creating the Domain Controller

### Creating the VM

- Created a VM and selected `Other Windows 64 (64-bit)` as the version. I also gave it the appropriate amount of resources. This can very depending on the host computer's resources.

![VBox 1](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/b1fd40ce-5af1-4c9b-a8d9-752c8f7fe450)

![VBox 2](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/d086df5a-c635-48a4-a324-73b87b5569a7)

- I then added an additional NIC. This one is used to connect to the Private Network.
  
![VBox 3](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/aec363f9-cc5c-4183-b8fe-5b7b477b983d)

### Installing Windows Server 2019

- I booted up the newly created DC VM, attached the Windows Server 2019 ISO and went through the installation process

![VBox 4](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/bde013fa-92f1-477f-bf08-3949523867dc)

- I made sure to install one of the `Desktop Experience` edition since the others don't have a GUI and are just a command line.
  
![VBox 5](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/de09e884-f23e-4786-9465-8301e365c6ae)


## Configuring the Domain Controller

### IP Addressing

- To begin the process of IP Addressing I navigated to `Network Connections` from within the `Control Panel`.
- Since this VM has two different NICs I found the internal one and named it `INTERNAL` to make it easy to distinguish.
- I then modified its TCP/IPv4 settings to the following.
- <b>Note:</b> this NIC did not have a Default gateway because the DC itself will act as the Default Gateway for the private network. It will route traffic from the clients, through the internal NIC and then through the public facing NIC for internet connectivity.

![VBox 6](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/783d34dc-9e48-43ec-a0c1-74ba20148892)

### Installing Active Directory 

- Using Server Manager I installed the `Active Direcotry Domain Services` role.

![VBox 7](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/93819c4c-5d44-4a95-968c-56277866e816)

- Using AD DS I created a new domain named `mydomain.com`.

![VBox 8](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/1262676c-767a-47e9-8431-55ab9a9e406a)

- Once AD DS was set up, I created a new `Organizational Unit` named `_ADMINS`.
- Here, I created an adminstrator account for myself (up to this point we had been using the default Administrator account).
- I switched over to that account for the remainder of the project.

![VBox 9](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/b4f81dba-7012-4919-8d02-2b2f0b9ac515)

### Setting up RAS/NAT

- Back in Server Manager, I added the `Remote Access` role and enabled `Routing` and `Remote Access Server (RAS)` on it.
- The goal with installing NAT is to allow the clients to sit on a private network but have internet access via the DC.

![VBox 10](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/088e6db0-8575-405e-8f59-f938aa1fd023)

- I then configured the newly installed Remote Access role to enable Network Address Translation (NAT).
  
![VBox 11](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/ffb2c3f4-76a4-4352-8905-668311b75c3a)

- I selected the public facing NIC to allow internet conectivity.

![VBox 12](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/1ae18d80-fa89-44b9-a26f-3d5275ae7413)

### Setting up DHCP

- Back in Server Manager, I added installed a `DHCP Server` role.

![VBox 13](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/a35aa082-1cfe-4a67-8b21-6ea271623a2e)

- I created a new scope for mydomain.com between `172.16.0.100-200`
- I set the Default Gateway as `172.16.0.1`. Again, this is the DC's IP Address and the DC will also server as the Default Gateway.

![VBox 14](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/0fe5ac17-794c-492a-bfc9-357656bcb924)

![VBox 15](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/6d802879-184f-488c-ac93-dc71498f5c73)

![VBox 16](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/b74b6f72-9e89-4412-a394-a5022ff4c537)

![VBox 17](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/276432ed-7190-4d6e-9e8f-ac6b811cbcde)


