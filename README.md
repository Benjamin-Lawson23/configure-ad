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

- Step 1: Create a Server and a Client virtual Machine
- Step 2: Ensure Connectivity between the client and Domain Controller
- Step 3: Install Active Directory
- Step 4: Create an Admin and Normal User Account in AD
- Step 5: Join Client to the domain
- Step 6: Setup Remote Desktop for non-administrative users on Client

<h2>Deployment and Configuration Steps</h2>

<h2>Step 1: In MS Azure, create a Server and a Client virtual Machine</h2>

1. Create a Resource Group named 'RGAD1'
2. Create a virtual Domain Controller (using Windows Server 2022) named 'DC-1'
3. Take a note of the Resource Group and Virtual Network (Vnet) that are created at this point
4. Set the Domain Controller’s NIC Private IP address to be static
5. Create a client Virtual Machine (VM) (using Windows 10) named 'Client-1'. Use the same Resource Group (RGAD1) and Vnet that was created previously
6. Make sure that both the Domain Controller and client VM are in the same Vnet (you can check the topology with Network Watcher)</p>
<br />

<p>
<img src="https://i.imgur.com/n67WFmt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<h2>Step 2: Ensure Connectivity between the client and Domain Controller</h2>

1. Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping), the ping should fail.
2. Login to the Domain Controller and enable ICMPv4 in on the local Windows Firewall with Advanced Security
3. Go back to Client-1,  the ping should now be successful
</p>
<br />

<p>
<img src="https://i.imgur.com/W316M7A.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<h2>Step 3: Install Active Directory</h2>

1. Login to DC-1 and 
2. Open Server Manager
3. Under 'Add Roles and Features', install Active Directory Domain Services
4. Click the yellow warning shield in the top right to promote as a DC: Setup a new forest as 'mydomain.com' (or anything you choose)
5. Restart the computer and log back into DC-1 as user, using the domain name before your user name: [mydomain.com]\username
</p>
<br />

<p>
<img src="https://i.imgur.com/XVZrvUh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<h2>Step 4: Create an Admin and Normal User Account in AD</h2>

1. Under Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) e.g. '_EMPLOYEES'
2. Create a new OU named e.g '_ADMINS'
3. Right click the _ADMINS OU and create a new user with username and password e.g.  “Jane Doe” 'jane_admin' 'Password'
4. Add 'jane_admin' to the “Domain Admins” Security Group
5. Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
6. You can now use jane_admin as your admin account from now on
</p>
<br />

<p>
<img src="https://i.imgur.com/y15SP0J.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<h2>Step 5: Join Client-1 to the domain (mydomain.com)</h2>

1. From the Azure Portal, set Client-1’s DNS settings to point toward the DC’s Private IP address: Networking -> click on the NIC -> DNS Servers -> Custom -> add DC-1's private IP address -> Save
2. From the Azure Portal, restart Client-1
3. Login to Client-1 (Remote Desktop) as the original local admin and join it to the domain:  Right click Start -> System -> Rename this PC -> Change -> Domain ->domain name (log in with domain admin (jane_admin account and the computer will restart) 
4. Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the 'Computers' container on the root of the domain
</p>
<br />

<p>
<img src="https://i.imgur.com/R70geng.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<h2>Step 6: Setup Remote Desktop for non-administrative users on Client-1</h2>

1. Log into Client-1 as mydomain.com\jane_admin and right click Start and open system properties
2. Click on 'Remote Desktop' on the right-hand side
3. Click on  'Select users that can remotely access this PC'
4. Add 'domain users' 
5. You can now log into Client-1 as a normal, non-administrative user
6. Any additional users created will all be able to log into client-1 

</p>
<br />

<p>
<img src="https://i.imgur.com/e7LfeQP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
<b>Congratulations! You have installed Active DirectoryE</b>
