# active_directory

<p>Active Directory stores information about objects on the network and makes this information easy for administrators 
  and users to find and use. Active Directory uses a structured data store as the basis for a logical, 
  hierarchical organization of directory information.</p>

<h4>Stage 1: Setup Resources in Azure</h4>
<ul>1.	Create the Domain Controller VM (Windows Server 2022) named “DC-1”. <h6>Take note of the Resource Group and Virtual Network (Vnet) that get created at this time.</h6></ul>
<ul>2.	Set Domain Controller’s NIC Private IP address to be static.</ul>
<ul>3.	Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1a</ul>
<ul>4.	Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher.</ul>

<h4>Stage 2: Ensure Connectivity between the client and Domain Controller</h4>
<ul>5.	Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping).</ul>
<ul>6.	Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall.</ul>
<ul>7.	Check back at Client-1 to see the ping succeed.</ul>

<h4>Stage 3: Install Active Directory</h4>
<ul>8.	Login to DC-1 and install Active Directory Domain Services.</ul>
<ul>9.	Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is).</ul>
<ul>10.	Restart and then log back into DC-1 as user: mydomain.com\labuser.</ul>

<h4>Stage 4: Create an Admin and Normal User Account in AD</h4>
<ul>11.	In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”.</ul>
<ul>12.	Create a new OU named “_ADMINS”</ul>
<ul>13.	Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”</ul>
<ul>14.	Add jane_admin to the “Domain Admins” Security Group.</ul>
<ul>15.	Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”.</ul>
<ul>16.	User jane_admin as your admin account from now on.</ul>

<h4>Stage 5: Join Client-1 to your domain (mydomain.com)</h4>
<ul>17.	From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address.</ul>
<ul>18.	From the Azure Portal, restart Client-1.</ul>
<ul>19.	Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart).</ul>
<ul>20.	Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain.</ul>
<ul>21.	Create a new OU named “_CLIENTS” and drag Client-1 into there (Step is not really necessary, just for organizational purposes. I guess I skipped this in the lab!)</ul>

<h4>Stage 6: Setup Remote Desktop for non-administrative users on Client-1</h4>
<ul>22.	Log into Client-1 as mydomain.com\jane_admin and open system properties.</ul>
<ul>23.	Click “Remote Desktop”</ul>
<ul>24.	Allow “domain users” access to remote desktop.</ul>
<ul>25.	You can now log into Client-1 as a normal, non-administrative user now.</ul>
<ul>26.	Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)</ul>

<h4>Stage 7: Create a bunch of additional users and attempt to log into client-1 with one of the users</h4>
<ul>27.	Login to DC-1 as jane_admin</ul>
<ul>28.	Open PowerShell_ise as an administrator</ul>
<ul>29.	Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)</ul>
<ul>30.	Run the script and observe the accounts being created</ul>
<ul>31.	When finished, open ADUC and observe the accounts in the appropriate OU</ul>
<ul>32.	attempt to log into Client-1 with one of the accounts (take note of the password in the script)</ul>



