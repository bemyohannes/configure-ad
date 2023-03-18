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
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<p>
The image below displays what we will be doing in this tutorial.

We will be creating two virtual machines (VMs) in the same Azure virtual network (VNET) called Client-1 and DC-1 (DC-1 is a domain controller, or server, that has Active Directory (AD) installed on it). The VNET will be created automatically when we create the virtual machines, in addition to a network interface card (NIC) for each VM. We will also setup the virtual NIC of DC-1 to have a static IP address, so that it does not change: it’s important to do this because DC-1 has AD installed on it and Client-1 will be joining DC-1 to get access to AD; therefore, it’s important for DC-1’s IP address to be static. </p>

<p>
<img src="https://i.imgur.com/EuLPCs3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<h3>Setup Resources in Azure</h3>

<p>Create a resource group in the Azure portal and then create the domain controller VM (Windows Server 2022) named “DC-1” and also the Client VM (Windows 10) named “Client-1.” The VNET and Subnet will be created automatically when you create the resource group. Just make sure they are both the same for both VM’s.</p>

<p>
<img src="https://i.imgur.com/2nWpVOx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>Next, set DC-1's NIC private IP address to be static.</p>

<p>
<img src="https://i.imgur.com/LKHm9RS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Setup Resources in Azure</h3>

The next step will be ensuring connectivity between the two VM’s (Client-1 and DC-1) by “pinging” in Command Prompt:

Login to Client-1 with Remote Desktop (RDC) using Client-1's public IP address and ping DC-1’s private IP address with “ping -t <ip address>” (perpetual ping). Note that connectivity failed, "Request Timed Out." 

<p>
<img src="https://i.imgur.com/HbJIAW7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>  
  
Login to DC-1 with (RDC) using its public IP address and enable ICMPv4 (the network protocol that ping uses, Internet Control Message Protocol) in the local windows Firewall by typing "firewall" in the search bar and selecting "Windows Defender Firewall with Advanced Security" > click "Inbound Rules" on top left > note ICMPv4 under "Protocol" heading and enable the three "Core Networking" rules by right-clicking and clicking "Enable Rule."  
  
<p>
<img src="https://i.imgur.com/Sjfj9xt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

Check back at Client-1 to see the ping succeed, now that ICMPv4 has been enabled in DC-1's Firewall. Note the change from "request Timed Out" to "Reply."
  
<p>
<img src="https://i.imgur.com/KD5Slwq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

 











<h3>Setup Resources in Azure</h3>













<h3>Setup Resources in Azure</h3>















<h3>Setup Resources in Azure</h3>
















<h3>Setup Resources in Azure</h3>

















<h3>Setup Resources in Azure</h3>






<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
