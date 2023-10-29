<p align="center">
<img src="https://github.com/landonbmartin/network-fileshare/assets/148168629/dae122fb-4c43-4288-af0c-724a4e0ea514" height=20% width=20%/>
</p>

<h1 align = "center">Network Fileshare and Permissions in Active Directory</h1>
File sharing and permissions are crucial for organizing resources and granting users proper file access in a business setting. With this lab showcasing Active Directory domain's file sharing and permissions functionality, it presupposes an existing Active Directory installation and configuration on a Domain Controller Virtual Machine, along with a connection to a Client Virtual Machine. If this setup is not in place, please refer to the tutorial <a href = "https://github.com/landonbmartin/configure-ad">here</a>

<br />

<h2>Environments and Technologies Used</h2>
<ul>
  <li>Microsoft Azure (Virtual Machines/Compute)</li>
  <li>Active Directory Domain Services</li>
  <li>Remote Desktop</li>
</ul>

<br />

<h2>Operating Systems Used</h2>
<ul>
  <li>Windows Server 2022</li>
  <li>Windows 10 Pro (22H2)</li>
</ul>

<br />

<h2>Steps</h2>

<h3>Creating Sample Fileshares with Various Permissions</h3>

<p>
  <ul>
    <li>Have the Domain Controller VM connect and log in as an admin (<b>mydomain.com\jane_admin</b>) and have the Client VM connect and log in as a random user generated through the <b>Powershell Script</b> during the configuration tutorial. In this example, my random user is named "cit.wex"</li>
    <li>In the Domain Controller VM, create the four folders below in the C:\Drive and set the permissions in these folders (by opening the folder's <b>Properties</b> and click on <b>Share</b> under the sharing tab)</li>
    <ul>
      <li><b>read-access</b> - Add the group Domain Users and set Permissions to Read</li>
      <li><b>write-access</b> - Add the group Domain Users and set Permissions to Read/Write</li>
      <li><b>no-access</b> - Add the group Domain Admins and set Permissions to Read/Write</li>
      <li><b>accounting</b> - Skip this folder for now</li>
    </ul>
    <li>The example below is setting the group and permissions for <b>read_access</b></li>
    <br>
    <img width="700" alt="read access example" src="https://github.com/landonbmartin/network-fileshare/assets/148168629/fae2ae9d-107a-41d0-b6cd-390ca3169640">
    </br>
  </ul>
</p>

<br />

<h3>Attempting to Access Fileshares</h3>

<p>
  <ul>
    <li>Head to the Client VM logged in as a random user and navigate to the shared folder through <b>File Explorer</b> and type in <b>\\dc-1</b></li>
    <br>
    <img width="700" alt="Navigate to dc-1" src="https://github.com/landonbmartin/network-fileshare/assets/148168629/76ad5c5a-dd53-48c0-b5c4-f815ce7d7c51">
    </br>
    <li>Attempt to access the folders through the Client VM. The only folder that should be inaccessible by how the permissions are set up should be <b>no-access</b></li>
    <br>
    <img width="700" alt="Access denied" src="https://github.com/landonbmartin/network-fileshare/assets/148168629/0e19bb32-d55a-4596-9c82-4ad525c0bcc4">
    </br>
  </ul>
</p>

<br />

<p>
  <ul>
    <li>Go back to the Domain Controller VM. Head to the Server Manager Board and go to <b>Active Directory Users and Computers</b> and create a new <b>Organizational Unit (OU)</b> and name it "_SECURITY_GROUPS"</li>
    <br>
    <img width="700" alt="Create Security Groups" src="https://github.com/landonbmartin/network-fileshare/assets/148168629/e345d32e-3eec-44b3-ad6e-027481a41d92">
    </br>
    <li>Inside the OU, create a <b>Group</b> and name it "ACCOUNTANTS"</li>
    <br>
    <img width="700" alt="Create Accountant Security Group" src="https://github.com/landonbmartin/network-fileshare/assets/148168629/537b2333-9148-47f9-9317-9cfeeaf68fa1">
    </br>
    <li>Find the <b>accounting</b> folder created in C:\Drive and add the ACCOUNTANTS group. Set its permissions to Read/Write</li>
    <br>
    <img width="700" alt="Accounting Permissions" src="https://github.com/landonbmartin/network-fileshare/assets/148168629/9844712f-f910-4208-9207-c3f05a11b88a">
    </br>
    <li>The user logged in to the Client VM should not have access to the accounting folder since it's not part of the Security Group. Log off the Client VM. Remember the username you used to log in to the Client as it's going to be a part of the ACCOUNTANTS group</li>
    <li>In the Domain Controller VM, go to the _SECURITY_GROUP OU, right click on the ACCOUNTANTS to open <b>Properties</b>, go to the <b>Members</b> tab and add the user as a member of the Group</li>
    <ul>
      <li>In the example below, the user "cit.wex" is used</li>
    </ul>
    <br>
    <img width="700" alt="Access cit wex to accounting" src="https://github.com/landonbmartin/network-fileshare/assets/148168629/e0f1b745-48cf-47f6-8fa0-edba71a1b501">
    </br>
    <li>Once the user is given access in the Domain Controller VM, sign out and sign back in to the Client VM with the random username selected. Navigate to the accounting folder in the shared C:\\Drive. It will now have access to the accounting folder because that user has been granted the appropriate permissions for the ACCOUNTANTS group</li>
  </ul>
</p>

<br />
