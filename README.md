<p align="center">
<img src="https://i.imgur.com/J7yCa3Z.jpeg" alt="ad logo"/>
</p>
<h1>Part 2 Deploying Active Directory</h1>
<h2>Description</h2>
<p>In this part of the Active Directory project, I set up Active Directory on the domain controller, create a domain administrator account, and connect the client virtual machine to the domain.</p>

<h2>Environments and Utilities Used</h2>

- Microsoft Azure
- Virtual Machines
- Remote Desktop Connection
- Active Directory

<h2>Operating Systems Used</h2>

- Windows Server
- Windows 10

<h2>Implementation Process</h2>
<p>
  <p>First, on the Domain Controller (DC), open the Server Manager dashboard if it isn’t already open.</p>
<img src="https://i.imgur.com/7NGVurs.png" alt="open server manager"/>
</p>
<p>
  <p>Click "Add roles and features," then select Next twice. On the server selection screen, you should see only one available option to choose from.</p> 
<img src="https://i.imgur.com/ewwBC2j.png" alt="Add roles and features"/>
</p>
<p>
  <p>Next, check the box for "Active Directory Domain Services" and then click "Add Features" when prompted.</p>
<img src="https://i.imgur.com/WT7r2no.png"  alt="Active Directory Domain Services"/>
</p>
<p>
  <p>Select "Active Directory Domain Services", then click Next three times. After that, check "Restart the destination server automatically if required" and click "Install."</p>
<img src="https://i.imgur.com/4qucZYU.png" alt="AD domain services"/>
</p>
<p>
  <p>Next, I need to promote this machine to a Domain Controller by setting it up as a new forest with the name "mydomain.com" (this name is flexible—mydomain.com is just easy to remember). To begin, I’ll click the flag icon with the yellow warning symbol in the top-right corner of the Server Manager dashboard, then choose "Promote this server to a domain controller."</p> 
<img src="https://i.imgur.com/FwbSFh0.png" alt="promote this server to a domain controller"/>
</p>
<p>
  <p>Then, choose "Add a new forest" and enter "mydomain.com" in the "Root domain name" field. After that, click Next to continue.</p>
<img src="https://i.imgur.com/PHmOK5O.png"  alt="Add a new forest"/>
</p>
<p>
  <p>On the next screen, enter a password of your choice in the provided fields, then click Next to proceed.</p>
<img src="https://i.imgur.com/heBHTsv.png" alt="DC password"/>
</p>
<p>
  <p>Leave the "Create DNS delegation" option unchecked and click Next. Continue clicking Next through the following screens until you reach the final one. There, click "Install." The system will automatically restart once the installation is complete.</p> 
<img src="https://i.imgur.com/HGId5vf.png" alt="DNS delegation unchecked"/>
</p>
<p>
  <p>After the restart—which may take a few minutes—sign back in using the username "mydomain.com\labuser" (assuming you followed the same naming convention), along with the password you set for labuser.
We include "mydomain.com" before the username because the machine is now a Domain Controller, and it needs to know whether you’re logging in as a domain user or a local account. By specifying "mydomain.com\labuser", you're telling the system to authenticate the user from the domain.

Now that you're signed in, we'll create an admin user. To do this, go to the Start menu and search for "Active Directory Users and Computers."</p>
<img src="https://i.imgur.com/V7atiLv.png"  alt="create admin user on AD"/>
</p>
<p>
  <p>In the Active Directory Users and Computers window, right-click on "mydomain.com", then select "New" > "Organizational Unit". Name this new OU _EMPLOYEES exactly as shown—this name is important because a future script will rely on it.

Repeat the same steps to create another Organizational Unit, this time named _ADMINS.</p>
<img src="https://i.imgur.com/hgxnsYE.png" alt="OU _EMPLOYEES and _ADMINS"/>
</p>
<p>
  <p>Then, add a new user to the "_ADMINS" group by right-clicking on "_ADMINS," selecting "New," and choosing "User." Complete the form as follows.</p> 
<img src="https://i.imgur.com/iNGPYkb.png" alt="New user to _admins"/>
</p>
<p>
  <p>Next, set a password for the user, then uncheck "User must change password at next logon" and check "Password never expires." (While this isn't recommended in a real-world environment, it's fine for this lab since it's just for practice.)</p>
<img src="https://i.imgur.com/t9bOoWc.png"  alt="_admins password"/>
</p>
<p>
  <p>Jane Doe has been added to the "_ADMINS" OU, but she doesn't have administrative privileges yet. To grant her admin rights, right-click her name, go to Properties > Member Of > Add..., type domain admins, click Check Names, then select OK, Apply, and OK again.</p>
<img src="https://i.imgur.com/aUzn7xU.png" alt="grant admin rigths to jane doe"/>
</p>
<p>
  <p>Now, log out of the Domain Controller and sign back in using Jane's credentials. Once logged in, access the client-1 VM (if you haven’t already), then join it to the domain by right-clicking the Start menu, selecting System > Rename this PC (advanced) > Change, choosing Domain, entering mydomain.com, and clicking OK.</p> 
<img src="https://i.imgur.com/TRTcIvk.png" alt="rename PC"/>
</p>
<p>
  <p>When prompted for an account with permission to join the domain, enter the admin credentials you used for Jane. After that, a welcome message confirming the domain join will appear, and the computer will begin restarting.</p>
<img src="https://i.imgur.com/lNKXXbH.png"  alt="Domain change"/>
</p>
<p>
  <p>After the restart, client-1 is now part of the domain. To verify this, go to the Domain Controller, open the Start search bar, type Active Directory Users and Computers, navigate to mydomain.com > Computers, and you should see client-1 listed there.</p>
<img src="https://i.imgur.com/LFSY3LE.png" alt="Users and Computers"/>
</p>
<p>
  <p>Next, create a new OU named "_CLIENTS" just like before. Then, drag and drop client-1 from the Computers container into the newly created "_CLIENTS" OU.</p> 
<img src="https://i.imgur.com/v9ZWucB.png" alt="create a OU _CLIENTS"/>
</p>
<H2>Active Directory is now deployed and ready to use!</H2>

<p>We have completed the key initial steps by installing Active Directory on our domain controller, which establishes the core infrastructure for managing our network resources. We've also created a domain admin account to handle administrative tasks securely. Additionally, we successfully joined the client virtual machine to the domain, allowing it to communicate and be managed within our network environment.

The next step involves automating user account creation using PowerShell scripts. This approach saves time and reduces errors compared to manual entry, especially when managing many users. Once the users are created, we will take control of user accounts and system settings by configuring Group Policies. Group Policy lets us enforce security rules, software installations, and other settings across all computers and users in the domain, ensuring consistency and compliance.</p>
