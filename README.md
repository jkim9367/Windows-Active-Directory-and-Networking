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

# Setting up Server 2019 VM:

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

Rename the PC to “DC” because the randomly assigned name is not easy to remember:

![4](https://user-images.githubusercontent.com/121040101/223148100-6348ca95-5c30-4d80-8cf8-a0b2dbc658e7.png)

Next, I installed active directory domain services and create domain:

![5](https://user-images.githubusercontent.com/121040101/223148326-57d0ac38-3793-42bc-8b66-f8c531a9d5a8.png)
![6](https://user-images.githubusercontent.com/121040101/223148329-b614e96f-9b03-440f-a93a-e6b904e0b284.png)
![7](https://user-images.githubusercontent.com/121040101/223148323-b3d6a2ad-3355-4216-aa22-598bd58f92b9.png)
![8](https://user-images.githubusercontent.com/121040101/223148325-570c11ed-4de4-4f37-8624-4d16b1e24ac8.png)
(Make sure to choose “active directory domain services”.) 

Do “post-deployment configuration”, which is creating a domain. 

In the deployment configuration, select “add a new forest” and root domain name as “mydomain.com” 

Press next with other options and install. 

It will automatically restart:

![9](https://user-images.githubusercontent.com/121040101/223148650-fa64ed4e-0064-4f5b-8c2c-4a49c378dfca.png)
![10](https://user-images.githubusercontent.com/121040101/223148653-6a0672ff-57f4-4510-bab3-b30ecdb0316f.png)
![11](https://user-images.githubusercontent.com/121040101/223148655-ec5395ac-1701-47d4-a514-639e3e1210d2.png)

Next, I created my own dedicate domain admin account instead of built in admin account. 

Go to start, windows administrative tools, active directory users and computers. 

In the active directory users and computers, I went to newly created domain, “mydomain.com” 

Right click, new, organizational units. To put the admin account. 

![12](https://user-images.githubusercontent.com/121040101/223148851-c42c8976-a837-4f2f-80b2-5e0862898462.png)

Created organizational unit “_ADMINS”

![13](https://user-images.githubusercontent.com/121040101/223148904-a09c987e-941a-4689-ab6c-2054debc0a63.png)

On the new “_ADMINS” folder, right click, new, user. Then create an admin account

![14](https://user-images.githubusercontent.com/121040101/223148989-8b00f9f9-144d-4645-b605-e5675cda3560.png)

I made a user account but need to make admin:

![15](https://user-images.githubusercontent.com/121040101/223149044-93a4a81c-15ac-47a8-98f4-d9685a845e49.png)

To give admin right By Adding the account in the domain admins group by right click “user”, “properties”, “member of”, add, type “Domain admins”, check names 

![16](https://user-images.githubusercontent.com/121040101/223149243-48664a1a-be20-4080-90a3-4a765cd3400f.png)

To user this admin account, logout and log in to the admin account 

# Installing RAS/NAT (remote access server netwrok address translation)

The purpose of RAS/NAT is to allow Windows 10 clients to be on private virtual netwrok through the domain controller

Diagram:
![Capture](https://user-images.githubusercontent.com/121040101/223150440-080fd438-8c61-434d-bf1e-f1a71e06a03e.PNG)

On the server manager, go the “add roles and features”. 

In the server roles, add remote access:

![17](https://user-images.githubusercontent.com/121040101/223150715-0bc33254-b2e3-47bc-8931-6bd4a7a7674e.png)

Also added routing:
![18](https://user-images.githubusercontent.com/121040101/223150865-a68c5440-2036-43ce-9063-702b3b2ecc8a.png)
Then installed:
![19](https://user-images.githubusercontent.com/121040101/223150934-9e0b708e-dffb-4ff8-b272-6d376ac87a27.png)

After installation is completed, go to server manager and go to tools, routing and remote access. 

Right click DC (local) and click configure and enable routing and remote access:

![20](https://user-images.githubusercontent.com/121040101/223151107-ff3527f9-abd5-4f29-a7e4-8d0212769212.png)

Select “NAT”:

![21](https://user-images.githubusercontent.com/121040101/223151166-e88d8050-7a69-4c70-952a-495f358fe756.png)

Next select “_INTERNET_” to use public interface to connect to the internet:

![22](https://user-images.githubusercontent.com/121040101/223151290-d4b27ddf-4830-4a6b-8cf4-595f5bd71710.png)
![23](https://user-images.githubusercontent.com/121040101/223151304-2b8022a6-da35-4194-b67a-50ba34486e6d.png)

Now there is a green arrow pointing up, this means I have correctly set up RAS/NAT. 
![24](https://user-images.githubusercontent.com/121040101/223151389-be5650fc-bf82-4368-9b87-f52afc4083dd.png)

# Setting up DHCP Server
Next, I am going to set up DHCP server on the domain controller to allow windows 10 clients to get an IP address to let them access the internet network. 

Go to add roles and features 

Select DHCP server 
