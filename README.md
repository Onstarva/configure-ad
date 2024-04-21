<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines. Note that all usernames and passwords should be your own and not generic names.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1
  Create a VM and resource group
- Step 2
  Create a Domain Controller (DC) and Client
- Step 3
  Install Active Directory on DC and Create an Admin user 
- Step 4
  Join Client VM to DC VM
- Step 5
  Setting up for all users to connect to Client VM
- Step 6 (Extra)
  Creating a bunch of random users to login into Client VM

<h2>Deployment and Configuration Steps</h2>

<p>
  
![Vnet](https://github.com/Onstarva/configure-ad/assets/166679644/aa3132aa-ddbd-4e0a-b82f-b62ca10f76b1)
![Dynamic to Static IP config for DC](https://github.com/Onstarva/configure-ad/assets/166679644/41bc7fab-3353-4d1d-992b-c3bb3a4b73b7)

</p>
<p>
First create your going to create two VMs, one named DC(domain controller) and the other will be your client. For DC VM will need to be 2 vcpus and the operating system will be Windows Server(latest version). Make sure to use a username and password that you will remember as you will be logging into both VMs. Hit Create and start creating your client VM. Use the same resource group, 2 vcpus and using Windows 10 operating system. Go to Network tab and under Vitual Network select the same network name as your DC Virtual network(vnet), then create the Client VM. Now go to VM->DC->Networking and click on Network Interface:dc-# in blue. Go to IP configurations and at the bottom you will see it says ipconfig1 is "dynamic" switch it to static with the three ... settings and it save. Once you created both VMs make sure they're both on the same network under Virtual Network/subnet.
</p>
<br />

<p>
  
![Active directory services install](https://github.com/Onstarva/configure-ad/assets/166679644/5de164d6-44ca-4fbc-9e80-daf15799e0cd)
![installing Active directory](https://github.com/Onstarva/configure-ad/assets/166679644/f3fccb01-f3ff-41f1-b779-06f739373027)


</p>
<p>
Next we'll install and setup Active Directory. Login into DC. Server Manager should be open but if not go to start menu and click Server Manager. Go to Add roles and Features, keep clicking next until you have a list of Server Roles. Mark Active Directory Domain Services and Add feature and continue installing. Once done go to top right screen of server manager dashboard and there will be a yellow triangle ! icon, click it and click Promote this server to a domain controller. A window will open and click add a new forest and name the domain, example "mydomain.com" and hit next and enter a password, go next and install. It will log you out, to log back in to DC. You will need to use your domain name followed by the username you created for DC. Example: "mydomain.com\johnsmith" for username.
<br />

<p>
  
![Active directory menu](https://github.com/Onstarva/configure-ad/assets/166679644/acb5bf3e-ddcf-439b-9814-a9c80b3a8e10)
![Active Directory Folders](https://github.com/Onstarva/configure-ad/assets/166679644/6bd4dd01-28ca-443f-9aa9-98733c5ed89e)
![Making an Admin user](https://github.com/Onstarva/configure-ad/assets/166679644/e9c22728-b0e0-4e20-8954-1a0250a72724)


</p>
<p>
Next you will want to create an admin user. Once logged in, go Server Manager->Tools->Active Directory Users and Computers. Right Click the server domain->New->Organization Unit and create a folder called _EMPLOYEES("_" is for easy tracking folder). Make another one called _ADMINS. Click _ADMINS folder and right click->NEW->USER and enter your name and user logon name. Set your password and finish. Next Right Click your new username and go to Properties->Memeber of->Add.. and type domain in the large open text space. Click check names to see a list of domains and click Domain Admins to add that group permission your account. Now log out of DC and log back in as your admin user account. Note you will be using the domain name followed by your username.
</p>
<br />

<p>
  
![Setting DNS for Client VM](https://github.com/Onstarva/configure-ad/assets/166679644/5af31360-118a-4de4-a079-01bc7c0ef638) 
![Connecting Client to DC](https://github.com/Onstarva/configure-ad/assets/166679644/80d5dfd1-5bb9-4c76-9fa6-32f66dc40add)


</p>
<p>
Now you will join Client VM to DC. Go back DC->Networking and copy the NIC Private IP:#. Go to VM->Client VM->Networking->Network Interface:Client-#->DNS Servers->Custom and paste the IP into DNS servers and save. Reset Client VM in Azure portal. Log in to Client VM. Right click windows->Settings->Rename this PC->Change-> click Domain and enter the Domain name you made from DC. Example "mydomain.com" and hit okay. A prompt will appear and enter the username and password of your admin account. The Client VM will restart. Now login to Client with you admin account since both DC and Client VM are now connected.
<p>
<br />

<p>

![Domain users](https://github.com/Onstarva/configure-ad/assets/166679644/33bf621c-d220-4344-8868-ed4a82ddc322)


</p>
<p>
Log into Client VM. Right click Start->Systems->Remote Desktop->User Accounts->Add..type in the big open text box Domain users->Check Name and click Ok.
</p>
<br />

<p>

![Script](https://github.com/Onstarva/configure-ad/assets/166679644/584e0e2f-f5b9-4d30-814c-cd125bfe1d32)
![Random users created](https://github.com/Onstarva/configure-ad/assets/166679644/3639188c-9681-4825-af32-30367c742616)
![Randomly Created Users](https://github.com/Onstarva/configure-ad/assets/166679644/eb1b3181-6d3b-4d55-a12c-f88367bf6883)


</p>
<p>
Next we will creat multiple users to login into Client VM. First log into DC as an admin. Open powershell as an admin by going search bar and enter powershell_ise Right click and open it as admin. At the top of powershell click new script and paste the contents of this script found here https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1 into the open space. You can edit how many accounts are made by changing the number at the top of the script. Click Run Script and a bunch of accounts will start to be made, it's 10000 by default if you don't change the number. Now if you go back into Active Directory _EMPLOYEES, refresh the folder and all the user names will appear in the folder. You can take any of these user profile names and login into Client VM with Password1 as the password.
</p>  
<br />
