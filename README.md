<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This demonstration outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


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
<img src="https://github.com/user-attachments/assets/f73a19bc-07d1-4ad3-9911-1e587482e4a8" width="95%" alt="Setting DC-1 IP to Static"/>
<img src="https://github.com/user-attachments/assets/d07036b9-bd6f-44b5-ba2d-aa544595b81e" width="95%" alt="Setting Client-1 DNS to DC-1"/>
</p>
<p>
We will want 2 different VMs active on the same network to deploy Active Directory. One will be the Domain Controller(DC-1), and one will be the Client(Client-1). Since DC-1 will be hosting the server that Client-1 will need access to, DC-1's IP address needs to be set static so it never changes. This will be done by entering dc-1's Virtual machine menu, then network settings, then Network interface / IP configuration, then ipconfig1 and changing Private IP address Allocation from Dynamic to Static. For client-1, navigate to the same Network interface settings menu and click DNS servers on the left pane, then select Custom and enter the dc-1's private IP address. To ensure connectivity, I will log into client-1 and ping dc-1's private IP address.
</p>
<br />

<h2>Install Active Directory</h2>
<p>
<img src="https://github.com/user-attachments/assets/7b549117-8ced-4f2b-852b-eb95676451c6" width="95%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/user-attachments/assets/d21f3c9c-ce8e-489d-891c-9a3acf90166c" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we will install Active Directory and set up a new forest as mydomain.com. We can do this within the Server Manager Dashboard on dc-1 by selecting "Add roles and features. Go through the installation wizard, selecting next until you can choose a role to install on the server, where you will check Active Directory Domain Services. Continue through the wizard to install, then we will promote the server to a domain controller by clicking the notification panel, selecting "Promote this server to a domain controller," then completing the wizard.
</p>
<br />

<h2>Create a Domain Admin User Within the Domain</h2>
<p>
<img src="https://github.com/user-attachments/assets/ed16c4fa-a4ac-4bb2-99aa-46667d100cdd" width="95%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/user-attachments/assets/cb9f9c18-316e-47f6-b9ab-125d8e236682" width="95%" alt="Disk Sanitization Steps"/>

</p>
<p>
In order to make a Domain Admin User in dc-1, we need to first create two Organizational Units, one called "_EMPLOYEES" and one called "_ADMINS" in Active Directory Users and Computers (ADUC). Within ADUC, right click mydomain.com and select New Organizational Unit and type "_EMPLOYEES" for the name, repeat again for "_ADMINS." We'll then create a new user within _ADMINS named Jane Doe, and add her to the group Domain Admins via the properties window. Now we are able to log in as Jane for administration tasks.
</p>
<br />

<h2>Join Client-1 VM to the Domain</h2>
<p>
<img src="https://github.com/user-attachments/assets/2b0040f0-bfcf-4b7f-99eb-a699f6d7e320" width="95%" alt="Disk Sanitization Steps"/>
</p>
<p>
To join Client-1 to the domain (mydomain.com) I will log into client-1, open "About your PC," click the option on the right "Rename this PC (advanced), and click the "Change" button to change its domain or workgroup. Check Domain under "Member of" and enter mydomain.com
</p>
<br />

<h2>Set Up Remote Desktop for Non-Admin Users on Client-1</h2>
<p>
<img src="https://github.com/user-attachments/assets/ce4cd1ae-6832-458d-9439-a76f0ec64887" width="95%" alt="Disk Sanitization Steps"/>
</p>
<p>
Logged into client-1 as Jane, open system properties (About your PC) and click Remote Desktop. I want to allow domain users to access client-1 through remote desktop, so I will click "Select users that can remotely access this PC," then click "Add," and enter "Domain Users."
</p>
<br />

<h2>Create Many Additional Users and Log Onto Client-1 With One of Them</h2>
<p>
<img src="https://github.com/user-attachments/assets/6cf5e111-4609-4ab3-8ac8-c90a36bd55cd" width="95%" alt="Disk Sanitization Steps"/>
</p>
<p>
Logged into DC-1 as Jane, I will open powershell_ise as an administrator, create a new file and run a script that will generate 1000 new accounts in the _EMPLOYEES OU. Each of these accounts will be able to be signed in to client-1 remotely. In my case I tested by logging in as bak.dax with the password that the script generated for all the accounts, and the login was successful!
</p>
<br />

<h2>Configure Group Policy To Lockout Accounts After Too Many Failed Attempts</h2>
<p>
<img src="https://github.com/user-attachments/assets/e8bd32cb-e94f-467b-9b03-f7c8df10ce8c" width="95%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we will configure Group Policy so that if a user makes too many failed attempts to log in, they will be locked out of their account. To do this, I access the Group Policy Management Console with my admin account logged into dc-1, navigate to "Default Domain Policy" under mydomain.com, right click it and click "Edit." In the Group Policy Management Editor, I can navigate to Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy, where I am able to make changes to what causes an account to lock out, as well as how long it stays locked along with some other options. I will set it to lock out accounts after 6 failed login attempts, and to stay locked for 10 minutes.
</p>
<br />

<h2>Lock Out An Account</h2>
<p>
<img src="https://github.com/user-attachments/assets/65b3768a-6ed6-427e-8c22-226898964178" width="95%" alt="Disk Sanitization Steps"/>
</p>
<p>
As an example, I will lock out bak.dax by attempting too many logins unsuccessfully to confirm that the Group Policy has taken effect.
</p>
<br />

<h2>Unlock the Account From Active Directory</h2>
<p>
<img src="https://github.com/user-attachments/assets/f191ca0a-868c-4a8c-bef3-7a1fb0d62921" width="95%" alt="Disk Sanitization Steps"/>
</p>
<p>
In this case bak.dax is locked out of his account for just 10 minutes, but in the case where for some reason a user is locked out for longer or needs to urgently log back in, its important to know how to unlock someone and reset their password. Back in dc-1, still logged in as an admin, I will find the user bak.dax in ADUC and go to his properties. Under Account I can check "Unlock Account" and click OK. Since he doesn't remember his password I will reset it by right clicking his user in ADUC and selecting "Reset Password." For this example I've reset it to "Password2."
</p>
<br />
