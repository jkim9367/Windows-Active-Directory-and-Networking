# Windows-Active-Directory-and-Networking

Summary:  

By using Virtual Machine, I created a domain controller and gave 2 NIC that connects to the internet and internal network. After, I installed active directory, created domain, configure NAT and routing. Using PowerShell script, I created 1000 clients in the active directory to simulate corporate environment. These clients was able access private network by setting up DHCP and login in to the the domain.

So, in this lab, I created the first domain machine on VM. Which will be my domain controller – housing active directory. I gave 2 NIC to this VM. One for connecting to the internet and the other one connected to the VM private network that clients are going to connect to. Then I will install server 2019 on it then assign IP address for the internal network. (External networks automatically have IP address from local network). 

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

Select DHCP server:

![25](https://user-images.githubusercontent.com/121040101/223151684-1b5ac96e-60ee-479c-919d-502cfe838e03.png)

Go to “tools”, “DHCP”, extend “dc.mydomain.com”, right click “IPv4”, click “new scoop”. 

I named the scope name as my IP address range which is “172.16.0.100-200”: 

![26](https://user-images.githubusercontent.com/121040101/223151846-bb4d6062-f335-4c14-9d97-8cc1915175e1.png)

I set my IP address range like this: 
![27](https://user-images.githubusercontent.com/121040101/223151917-f3258db4-e00e-48b8-b663-5e2a2e6ea136.png)

For the router (default gateway)

This forward traffic from the client to the internet. So, I put domain controller’s IP address. 

![28](https://user-images.githubusercontent.com/121040101/223152133-5734db36-f073-4af6-9d13-165ec1a4b91e.png)

Next, right click “dc.mydomain.com”, click “authorize” and right click again and refresh. 

Now I can see IPv4 and IPv6 have turned green and it's working:

![29](https://user-images.githubusercontent.com/121040101/223152216-2bd8d920-ee36-424a-9d18-890e345ed825.png)

# Using PowerShell script to create 1,000 users in the Active Directory

Now I am going to use PowerShell script to create Windows 10 client users in the Active Directory instead of doing it manually.

I downloaded my “1_CREATE_USERS.ps1” and "names.txt" files on the desktop

I opened "Windows PowerShell ISE" as an administrator and open the script, “1_CREATE_USERS.ps1”:
![30](https://user-images.githubusercontent.com/121040101/223152611-6c37c414-665e-403b-98c4-8ea7075b6912.png)

On command line, put “Set-ExecutionPolicy unrestricted” and click “yes to all”: 
![31](https://user-images.githubusercontent.com/121040101/223152781-62ccb9ef-0c7e-4d68-a749-096e40e1d982.png)

Run the script:
![32](https://user-images.githubusercontent.com/121040101/223153852-40a42e5a-333a-4abb-8c03-8f992fa47d1c.png)

Through this script, I automatically created a group called “_USERS” and inside of this group, there are 1000 users' accounts:
![33](https://user-images.githubusercontent.com/121040101/223153943-458a44f2-e93d-405b-9520-b597bee7d305.png)

# Create Wundows 10 VM and login as one of the users.

When creating a new VM, I selected the internal network for the network:
![34](https://user-images.githubusercontent.com/121040101/223154333-ddfab671-576b-4fd8-b16e-08c67c914fd1.png)

Make sure to select windows 10 pro. This allows you to join the internal network. 
![35](https://user-images.githubusercontent.com/121040101/223154382-948e7ac3-dc88-4aef-9173-be5fc12fec17.png)

# Checking if Windows 10 VM has connected to internal network

On the windows 10 VM, go to command prompt. Type “ipconfig” to make sure the windows 10 VM have default gateway and network is connected
![36](https://user-images.githubusercontent.com/121040101/223154667-7b489224-9882-486e-aec9-797bad86c68b.png)

I pingged google.com to make sure the VM is connected to the internet:
![37](https://user-images.githubusercontent.com/121040101/223154780-12ae80bc-b25f-474b-854d-eb54217423da.png)

The Ping was successful. This means that connectivity to the default gateway (domain controller) is working, and domain controller is successfully natting it and forwarding it out to the internet. Then, it is able to ping back to the Client1 (Windows 10 VM). 

Diagram:

![ping](https://user-images.githubusercontent.com/121040101/223159736-daf47e30-4c0c-40de-b241-3e784306523d.PNG)


Mydomian.com works too:
![38](https://user-images.githubusercontent.com/121040101/223154849-a1781fd5-479c-4f2f-8ee4-89439aa2f505.png)

# Joining the domain (mydomain.com)

I am going to change the name of the device and join the domain (mydomain.com): 

![39](https://user-images.githubusercontent.com/121040101/223156429-7d6e8b7b-3efe-4697-b3a6-e7115506d1f1.png)

After changing to name and joining the domain, it asks you to login. I can login as any account within the active directory, which I created with the PowerShell script. 

![40](https://user-images.githubusercontent.com/121040101/223156717-fdbf7e13-54de-4117-9f1f-fc663079f5ef.png)

Once successfully login, it will restart:
![41](https://user-images.githubusercontent.com/121040101/223156801-9fa5f002-6e6c-4764-8cf6-fbe2758cabf2.png)

If I go back to domain controller (Server 2019 VM), go to "DHCP", "address lease", I can see the Windows 10 VM. Once logged in to the mydomain.com, the DHCP server automatically assigned an IP address:

![42](https://user-images.githubusercontent.com/121040101/223157404-afd697a1-f056-4a19-ba8b-5575937d3e07.png)

If I go to active directory users and computers, in the mydomain.com, computers, the “CLIENT1” is member of the domain because I was able to join the domain. 

With “CLIENT1” I can log in with any of 1000 user accounts. But if I delete “CLIENT1” I would not be able to login with that device. 

![43](https://user-images.githubusercontent.com/121040101/223157735-7b7391ee-0a0f-456b-8bbd-81487d1e08a9.png)


# Check if I am in the Domain
If I go to windows 10 VM and select login as other user. It said “sign in to: MYDOMAIN” 

![44](https://user-images.githubusercontent.com/121040101/223157798-82efd4b1-9b17-44b4-8ecd-12daca927f66.png)

This means that I have successfully joined the domain. 

I can double check by going to Command Prompt and type "whoami":
![45](https://user-images.githubusercontent.com/121040101/223158373-dc0759b7-a40f-4836-a0e1-56e7686da774.png)


This concludes the lab.
