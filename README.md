# Active Directory Lab

## Description

For this lab I created a virtualized enviorment using VirtualBox. The project consisted of two virutal machines: 

The first virtual machine would host Windows Server 19. It would also have two seperate NICs. One for connecting to the internet and one for internal connections. I als ran a PowerShell script to automate the process of adding 1k+ users to the domain. The services running on the server are:
* <b> AD DS: </b> I installed Active Directory for user management
* <b> NAT: </b> NAT was used for connectivity to the clients as well as providing said clients access to the internet.
* <b> DHCP: </b> I installed DHCP so that each client could have an IP Address assigned.


The second virtual machine would host one test client device with an internally connecting NIC. This client would be used to ensure the server is properly configured.

## Topology

<img src="https://i.imgur.com/w2YHkGD.png" />
