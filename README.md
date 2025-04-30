<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This demonstration outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create Domain Controller and Client VMs in Azure
- Install Active Directory
- Create a Domain Admin User Within the Domain
- Join Client-1 VM to the Domain
- Set Up Remote Desktop for Non-Admin Users on Client-1
- Create Many Additional Users and Log Onto Client-1 With One of Them
- Configure Group Policy To Lockout Accounts After Too Many Failed Attempts
- Lock Out An Account
- Unlock the Account From Active Directory
- Enabling and Disabling Accounts

<h2>Deployment and Configuration Steps</h2>
<h2>Create Domain Controller and Client VMs in Azure</h2>
<p>
<img src="https://github.com/user-attachments/assets/f73a19bc-07d1-4ad3-9911-1e587482e4a8" width="45%" alt="Setting DC-1 IP to Static"/>
<img src="https://github.com/user-attachments/assets/d07036b9-bd6f-44b5-ba2d-aa544595b81e" width="45%" alt="Setting Client-1 DNS to DC-1"/>
</p>
<p>
We will want 2 different VMs active on the same network to deploy Active Directory. One will be the Domain Controller(DC-1), and one will be the Client(Client-1). Since DC-1 will be hosting the server that Client-1 will need access to, DC-1's IP address needs to be set static so it never changes. This will be done by entering dc-1's Virtual machine menu, then network settings, then Network interface / IP configuration, then ipconfig1 and changing Private IP address Allocation from Dynamic to Static. For client-1, navigate to the same Network interface settings menu and click DNS servers on the left pane, then select Custom and enter the dc-1's private IP address. To ensure connectivity, I will log into client-1 and ping dc-1's private IP address.
</p>
<br />

<h2>Install Active Directory</h2>
<p>
<img src="https://github.com/user-attachments/assets/7b549117-8ced-4f2b-852b-eb95676451c6" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/user-attachments/assets/d21f3c9c-ce8e-489d-891c-9a3acf90166c" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we will install Active Directory and set up a new forest as mydomain.com. We can do this within the Server Manager Dashboard on dc-1 by selecting "Add roles and features. Go through the installation wizard, selecting next until you can choose a role to install on the server, where you will check Active Directory Domain Services. Continue through the wizard to install, then we will promote the server to a domain controller by clicking the notification panel, selecting "Promote this server to a domain controller," then completing the wizard.
</p>
<br />

<h2>Create a Domain Admin User Within the Domain</h2>
<p>
<img src="https://github.com/user-attachments/assets/ed16c4fa-a4ac-4bb2-99aa-46667d100cdd" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/user-attachments/assets/cb9f9c18-316e-47f6-b9ab-125d8e236682" width="80%" alt="Disk Sanitization Steps"/>

</p>
<p>
In order to make a Domain Admin User in dc-1, we need to first create two Organizational Units, one called "_EMPLOYEES" and one called "_ADMINS" in Active Directory Users and Computers (ADUC). Within ADUC, right click mydomain.com and select New Organizational Unit and type "_EMPLOYEES" for the name, repeat again for "_ADMINS." We'll then create a new user within _ADMINS named Jane Doe, and add her to the group Domain Admins via the properties window. Now we are able to log in as Jane for administration tasks.
</p>
<br />

<h2>Join Client-1 VM to the Domain</h2>
<p>
<img src="https://github.com/user-attachments/assets/2b0040f0-bfcf-4b7f-99eb-a699f6d7e320" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To join Client-1 to the domain (mydomain.com) I will log into client-1, open "About your PC," click the option on the right "Rename this PC (advanced), and click the "Change" button to change its domain or workgroup. Check Domain under "Member of" and enter mydomain.com
</p>
<br />

<h2>Set Up Remote Desktop for Non-Admin Users on Client-1</h2>
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<h2>Create Many Additional Users and Log Onto Client-1 With One of Them</h2>
<p>
<img src="h" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<h2>Configure Group Policy To Lockout Accounts After Too Many Failed Attempts</h2>
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<h2>Lock Out An Account</h2>
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<h2>Unlock the Account From Active Directory</h2>
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<h2>Enabling and Disabling Accounts</h2>
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
