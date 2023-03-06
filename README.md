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
![diagram](https://user-images.githubusercontent.com/121040101/223145428-ce58da72-0611-4a6b-b32a-9353e6886f19.PNG)

Setting up Server 2019 VM:

I created server 2019 virtual machine named “DC” (Domain Controller) first on the VirtualBox. 

When creating “DC” VM, in the network setting, I will create the first adapter as NAT and second one as internal network:

![0 1](https://user-images.githubusercontent.com/121040101/223146336-2cb0b20e-1fb9-4f96-9f6d-c38aa0e78678.png)
![0 2](https://user-images.githubusercontent.com/121040101/223146352-30aa1afa-93d3-4216-836d-5d60f520e2cb.png)

Installing Server 2019 VM:

![1](https://user-images.githubusercontent.com/121040101/223146770-299835bc-7453-454f-b68c-86e7ffda71eb.png)

Changed the name of the network. 
I changed the Internet as "_INTERNET_" and internal network as "_internal":

![2 changed name of the networks and setting up IP address of internal network](https://user-images.githubusercontent.com/121040101/223147412-71e7954f-40ac-4719-8bd0-8661d15e83d8.png)

Next, I assigned an IP address to the internal network:

![3 assigned ip address for internal network](https://user-images.githubusercontent.com/121040101/223147813-475ae18f-b36d-42c2-ba68-3b91ed13b5dc.png)

I am not assigning default gateway because domain controller itself is serving as default gateway. 

