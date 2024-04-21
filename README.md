<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


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
- Install Active Directory on DC and join the Client to the domain 
- Step 4
  Create an example of multiple users able to login into the DC with powershell script

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
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next you will want to create an admin user. Once logged in, go Server Manager->Tools->Active Directory Users and Computers. 

</p>
<br />
