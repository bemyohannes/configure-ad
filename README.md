<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
Hey and welcome back! This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Ensure Connectivity Between the Client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in Active Directory
- Join Client-1 to Your Domain (mydomain.com)
- Setup Remote Desktop for Non-Administrative Users on Client-1
- Create Additional Users and Attempt to Login to Client-1 With One of the Users

<h2>Deployment and Configuration Steps</h2>

<p>
The image below displays what we will be doing in this tutorial.

We will be creating two virtual machines (VMs) in the same Azure virtual network (VNET) called Client-1 and DC-1 (DC-1 is a domain controller, or server, that has Active Directory (AD) installed on it). The VNET will be created automatically when we create the virtual machines, in addition to a network interface card (NIC) for each VM. We will also setup the virtual NIC of DC-1 to have a static IP address, so that it does not change: it’s important to do this because DC-1 has AD installed on it and Client-1 will be joining DC-1 to get access to AD; therefore, it’s important for DC-1’s IP address to be static. </p>

<p>
<img src="https://i.imgur.com/EuLPCs3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<h3>Setup Resources in Azure</h3>

Create a resource group in the Azure portal and then create the domain controller VM (Windows Server 2022) named “DC-1” and also the Client VM (Windows 10) named “Client-1.” The VNET and Subnet will be created automatically when you create the resource group. Just make sure they are both the same for both VM’s.

<p>
<img src="https://i.imgur.com/2nWpVOx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>Next, set DC-1's NIC private IP address to be static.</p> <br />

<p>
<img src="https://i.imgur.com/LKHm9RS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Ensure Connectivity Between the Client and Domain Controller</h3>

The next step will be ensuring connectivity between the two VM’s (Client-1 and DC-1) by “pinging” in Command Prompt:

Login to Client-1 with Remote Desktop (RDC) using Client-1's public IP address and ping DC-1’s private IP address with “ping -t <ip address>” (perpetual ping). Note that connectivity failed, "Request timed out." 

<p>
<img src="https://i.imgur.com/HbJIAW7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>  
  
Login to DC-1 with (RDC) using its public IP address and enable ICMPv4 (the network protocol that ping uses, Internet Control Message Protocol) in the local windows Firewall by typing "firewall" in the search bar and selecting "Windows Defender Firewall with Advanced Security" > click "Inbound Rules" on top left > note ICMPv4 under "Protocol" heading and enable the three "Core Networking" rules by right-clicking and clicking "Enable Rule." 
<br />  
  
<p>
<img src="https://i.imgur.com/Sjfj9xt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

Check back at Client-1 to see the ping succeed, now that ICMPv4 has been enabled in DC-1's Firewall. Note the change from "Request timed out" to "Reply."
  
<p>
<img src="https://i.imgur.com/KD5Slwq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

 <h3>Install Active Directory</h3>

<p>From within DC-1, Server Manager Dashboard should have opened automatically when you logged in. Now, click “Add roles and features” > click Next > Next > Next > select “Active Directory Domain Services” > click Add Features > click Next through to Install and Install and then Close.</p>

<p>
<img src="https://i.imgur.com/om3JZ1L.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

Next, click the yellow notification symbol on the tool bar on top right and click "Promote this server to a domain controller"

<br />
<p>
<img src="https://i.imgur.com/2UNPDpP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

Then, select "Add a new forest" in the Deployment Configuration window that opens and type "mydomain.com" as the "Root domain name" and click Next > provide a password > Next through to Install > Install
  
<p>
<img src="https://i.imgur.com/f4Jgjqo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
<br />
  
DC-1 will restart on its own. Log back in to DC-1 using the domain name and password created above, mydomain.com\labuser. 

<p>
<img src="https://i.imgur.com/WVrNHbF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
  
<h3>Create an Admin and Normal User Account in Active Directory</h3>

From DC-1, you can access Active Directory by searching "Active Directory Users and Computers" in the search bar or by clicking Tools on the tool bar of Server Manager Dashboard and clikcing "Active Directory Users and Computers," either way is fine.

<br />  
<p>
<img src="https://i.imgur.com/t8lBIr4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<br />

In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) named “_EMPLOYEES” and second OU named “_ADMINS” by right-clicking on “mydomain.com” in the top left task bar > navigate to New > click Organizational Unit (a folder within AD, essentially).

<br />
<p>
<img src="https://i.imgur.com/yRMgoK8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” by clicking _ADMINS OU in the left task bar and right-clicking in the empty space of the OU > navigate to New > click User

<p>
<img src="https://i.imgur.com/afI1cBz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

Now, add jane_admin to the “Domain Admins” Security Group by right-clicking Jane Doe from within the _ADMINS OU > Properties > Member Of > Add > type “domains” and click Check Names > click Domain Admins > OK > Apply and OK

<p>
<img src="https://i.imgur.com/8FnYxd0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

Close the Remote Desktop Connection to DC-1 and log back in as “mydomain.com\jane_admin”  
  
<p>
<img src="https://i.imgur.com/QIaNGmZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>  
<br />
Use jane_admin as your admin account from now on

  
<h3>Join Client-1 to Your Domain (mydomain.com)</h3>

From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address by going into the DC-1 VM > click Networking under Settings in the left task bar > copy the NIC Private IP address

<p>
<img src="https://i.imgur.com/9rioAK8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

Next, go into Client-1 VM in the portal > Networking under Settings > click the Network Interface client number

<p>
<img src="https://i.imgur.com/O6Qe2py.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

Click DNS servers under Settings > select Custom DNS servers > paste DC-1’s private IP address that you copied before as the DNS server > click Save above

<p>
<img src="https://i.imgur.com/oBov3wl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

From the Azure Portal, restart Client-1

<p>
<img src="https://i.imgur.com/tz54Vq8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart): right-click the Windows icon from the task bar > System > Rename this PC under Related Settings on the right > Change > click Domain under Member of > type “mydomain.com” > OK > enter the Jane_Admin account information to join the domain > OK >  
  
<p>
<img src="https://i.imgur.com/MbEkXQw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
  
Client-1 will need to be restarted to join the domain. 

<h3>Setup Remote Desktop for Non-Administrative Users on Client-1</h3>

Now, log back into Client-1 as mydomain.com\jane_admin and allow “domain users” access to remote desktop: right-click Windows icon > System > Remote desktop, on the left > click "Select users that can remotely access this PC" under User accounts > Add > type "domain users" > click Check Names > OK > OK
  
<p>
<img src="https://i.imgur.com/7JxDYXh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

You can now log into Client-1 as a normal, non-administrative user.

<h3>Create Additional Users and Attempt to Login to Client-1 With One of the Users</h3>

You should already be logged into DC-1 as jane_admin; to verify you can type "whoami" and "hostname" into Command-line.

<p>
<img src="https://i.imgur.com/MPuN08n.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

Open PowerShell_ise as an administrator by searching "powershell ISE" in the task bar and clicking "Run as administrator" on the right. 
  
<p>
<img src="https://i.imgur.com/cuRuISD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
Create a new file in PowerShell and paste the contents of the following script into it: PowerShell script. Then, run the script by clicking the green symbol above the script and observe the accounts being created.  
  
<p>
<img src="https://i.imgur.com/Potj0Kq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
  
Open Active Directory Users and Computers (ADUC), right-click the _EMPLOYEES OU, click Refresh and observe the created accounts. 
  
<p>
<img src="https://i.imgur.com/NKwQQS1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>  
  
<p>
<img src="https://i.imgur.com/TphSEOS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
Now, log out of Client-1 and attempt to log back in with one of the accounts (take note of the password in the script).
  
<p>
<img src="https://i.imgur.com/O7exBfU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
 
<p>
<img src="https://i.imgur.com/cZz70SV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>  
  
Great! You should have been able to log in; to verify you can always use Command-line: 

<p>
<img src="https://i.imgur.com/wxnbKlx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
This concludes the tutorial. Congratulations!
