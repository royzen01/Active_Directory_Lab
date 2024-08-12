# Active Directory Lab

## Description

For this lab I created a virtualized Active Directory enviroment using VirtualBox. The project consisted of two virtual machines: 

The first virtual machine will be the Domain Controller (DC) and it will host Windows Server 19. It will also have two seperate NICs. One for connecting to the internet and one for internal connections. I also ran a PowerShell script to automate the process of adding 1k+ users to the domain. The services running on the server are:

* <b> AD DS: </b> I installed Active Directory for user management
* <b> NAT: </b> NAT was used for connectivity to the clients as well as providing said clients access to the internet.
* <b> DHCP: </b> A DHCP server will be needed in order to assign IP Addresses to each client.

The second virtual machine will run Windows 10. This VM will serve as a test client device and its NIC will connect to the private network. This client would be used to ensure the server is properly configured.

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

### Adding users to AD

- I used a PowerShell script and a text file with a list of names to automate the process of adding users to Active Directory. This added 1k+ users to the domain.

![VBox 18](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/8bb35f30-ff0c-4876-bb7c-e575f69055a0)

![VBox 18 5](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/5bb56e33-c7ed-4f4d-bde3-9851850834c4)

![VBox 19](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/5054eb32-2f3e-4d77-bcde-1b7cb2a642a1)


## Creating a test client

### Creating the VM

![VBox 20](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/549a8b63-4278-4a54-b070-2ed16b6b7477)

- For the client, I switched its NIC to connect only to the internal network. It will connect to the DC's internal NIC, get an IP assigned via DHCP and have internet connectivity thanks to NAT.

![VBox 21](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/a1f6db6b-1878-412e-b763-eb30b1de403e)

## Installing Windows 10

![VBox 22](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/2093dbd2-21ea-4228-9deb-dd86698ec87a)

![VBox 23](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/dd1a8de6-5e2b-4531-a162-ed0310ca061e)

- I created a default user but later we will test if the users we added to the domain earlier can sign in into this client.

![VBox 23](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/63da368c-b72b-4534-907a-1ba4ea718d7d)

## Testing connectivity

- I ran a simple ipconfig and ping commands to ensure the device was assigned an IP by the DC and that it was able to reach the internet.

![VBox 24](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/e647421b-8d22-4152-bd5c-aa682827f9f4)

![VBox 25](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/136fe0a5-5126-436e-a8ff-3dcf9f9e5095)

![VBox 26](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/d0c08979-e4c9-4de2-89f3-299837d0339e)

- I also made sure the client had been properly added to the domain.

![VBox 29](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/017fec5d-00c7-4a44-bc2f-892a346e341a)

## Login test

- To finish it off, I switched back over to the client machine and attempted to log in using one of the random users we added to AD using the script. If everything has been properly configured, we should be able to log into any machine using any username that was added to the `mydomain.com` domain.

![VBox 27](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/97ec97b4-fd20-4e65-a206-bed12dbc661f)

![VBox 28](https://github.com/royzen01/Active_Directory_Lab/assets/13005742/5b3f53ba-7d9c-49f6-9c15-fa981630f37b)


### Conclusion

This project gave me a much better understanding on how Active Directory functions within a private network. Creating something akin to a mini corporate network was good practice and it helped me visualize how connections flow in a private network. Since Active Directory is an industry standard it is important to understand how its components interact with each other, and this project was a great way of doing so. 

I will be exploring more complex topologies and projects that involve AD as a way of becoming more familiar with it.





