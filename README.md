# Azure_Active_Directory
Procedure on how to setup and deploy Active Directory on Azure.


<h2>Environments and Technologies Used</h2>

**Technologies and Servies**
- Microsoft Azure
- Active Directory Domain Services


**Programing Language**
- PowerShell

**Environment/Operating System**
- Windows Server 2022
- Windows 10

<h2>High-Level Deployment and Configuration Steps</h2>

**Step 1: Creating Vitual Boxes**

- Created a respository and named it "Active-Directory"
- Created a new virtual network and named it "domain"
- Created two new virtual machines and using the same respository and virtual network.
  - One machine will be named "DC-1" using Windows Server 2022.
  - One machine will be named "Client-1" using Windows 10.
  - Both machines will have a username "Tech" and password "Password1234".
	
**Step 2: Set DNS**

- Select "DC-1" in the home page of Azure.
- Select "Network" then "Network Setting" on the left hand column.
- Select the NIC card name and select "ipconfig" in the bottom.
- Set Private IP as Static IP.
- Select "Client-1" in the home page of Azure.
- Select "Network" then "DNS Setting" on the left hand column.
- Input the Private IP of "DC-1" and save it.

**Step 3: Verify Connection**

- Copy IP address of "DC-1" on Azure and login to the virtual machine by remote desktop connection.
- On Windows search bar, enter "Run" and type wf.msc 
- On Windows Firewall, select ---- and under each tab turn off the firewall.
- Copy IP address of "Client-1" on Azure and login to the virtual machine by remote desktop connection.
- Open command prompt and ping "DC-1" private IP address.

**Step 4: Installed Active Directory**

- Open Server Manager.
- Under "Manage", select "Add Roles and Features".
- Follow the steps and add "Active Diretory Domain Services" role.
- Prompted for deployment configuration, select "Add a new forest" and create a domain name (e.g. mydomain.net).
- Continue with the configuration and restart DC-1.
- Log back in as "mydomain.net\Administrator".

**Step 5: Setup User Accounts in Active Directory**

- Open "Active Direcoty Users and Computer" in DC-1.
- Create new Organiztional Unit call "_EMPLOYEES".
- Create a new employee name "Jane Doe" with user name "jane.doe" and a password "Password1234".
- Add "jane.doe" to the "Domain Admins" Security Group.
- Log out of DC-1 and sign in with "mydomain.net\jane.doe".

**Step 6: Add Client-1 to Domain**

- Open "Advance System Settings" in Client-1.
- Under tab "Computer Name", click "Change".
- Under "Member of" select "Domain" and type your domain name(mydomain.net).
- Join the domain(mydomain.net) and restart your computer.
- Verify Client-1 is in domain by logging in DC-1, open Active Directory Users and Computer under "Computers".

**Step 7: Remote Access Domain Users**

- Login to "Client-1" as Jane Doe (mydomain.net\jane.doe)
- In Settings, go to "Remote Desktop Settings".
- Select "Select Users for Remote Access"
- Type "Domain Users" and select Check Names and apply the setting.

**Step 8: Create Users**

- Log in DC-1 as "mydomain.net\jane.doe".
- Open PowerShell_ise as an adminstrator.
- Run a script call Generate-Names-Create-Users.ps1 [(Josh Madokor's source code)](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
- Once finished, open Active Directory Users and Computer and observe new users under "_EMPLOYEES"
- Log into Client-1 with one of the new users created.

<h2>Final Notes</h2>

In conclusion, this project successfully demonstrated the implementation of Active Directory within a cloud-based 
infrastructure using Microsoft Azure. From creating virtual machines and configuring networks to deploying users 
into the domain, the project reflected typical processes found in an enterprise IT environment. Virtual machines 
were configured, a domain was established, and user accounts were created and tested for domain access.

<br />
This project not only reinforced core concepts of networking and directory management, but also showcased the
capabilities of Azure in replicating traditional on-premises environments in the cloud. This project has provided
me with a deeper understanding of the processes and procedures involved in deploying and implementing Active Directory,
as well as utilizing cloud infrastructure like Azure.
