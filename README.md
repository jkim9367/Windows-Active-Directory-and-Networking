# Windows-Active-Directory-and-Networking

Summary:  

Created the first domain machine on VM. Which will be my domain controller – housing active directory. I gave 2 NIC to this VM. One for connecting to the internet and the other one connected to the VM private network that clients are going to connect to. Then I will install server 2019 on it then assign IP address for the internal network. (External networks automatically have IP address from local network). 

After, I named the server, installed active directory, created a domain, configure NAT and routing, so client on private network can access the internet through the domain controller, I also setup DHCP on domain controller so when Windows 10 VM is created and login as one of the clients, it will automatically be assigned with an IP address. 

Last thing before created client VM (Windows 10), I ran PowerShell script that automatically create 1,000 users in active directory. 

Downloaded links:
Oracle VirtualBox: https://www.virtualbox.org/wiki/Downloads 

Windows 10 ISO: https://www.microsoft.com/en-us/software-download/windows10 

Server 2019 ISO: https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019 

Diagram of the whole project:


Setting up Server 2019 VM:
