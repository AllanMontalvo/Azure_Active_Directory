<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

# Azure Active Directory
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
<img height="60%" width="60%" alt="Resource Created" src="https://github.com/user-attachments/assets/57a1da6c-9030-4551-bbc3-5f381ae3c618" />

- Created a new virtual network and named it "domain"
<img height="60%" width="60%" alt="Virtual Network Created" src="https://github.com/user-attachments/assets/bb9293c6-5d7c-4990-b9a4-dbe97cecfca3" />

- Created two new virtual machines and using the same respository and virtual network.
  - One machine will be named "DC-1" using Windows Server 2022.
  - One machine will be named "Client-1" using Windows 10.
  - Both machines will have a username "Tech" and password "Password1234".
<img width="335" alt="DC-1 created 1" src="https://github.com/user-attachments/assets/e06a4b67-ab7b-4baf-a2f2-fe6d54791555" />
<img width="355" alt="DC-1 created 2" src="https://github.com/user-attachments/assets/96912e9c-2087-4542-82da-6e7b6fefd505" />

<img width="355" alt="Client-1 created 1" src="https://github.com/user-attachments/assets/bfb44b15-21f1-4656-9927-05e26e96f042" />
<img width="355" alt="Client-1 created 2" src="https://github.com/user-attachments/assets/65dc5942-76c8-4c40-80fd-974e2141b0d3" />


	
**Step 2: Set DNS**

- Select "DC-1" in the home page of Azure.
- Select "Network" then "Network Setting" on the left hand column.
- Select the "Network Interface" name and select "IP configurations" under setting on the left hand column
- Select "ipconfig1" in the bottom.
- Set Private IP as Static IP and copy the Private IP address.
<img width="745" alt="DNS Setting DC-1" src="https://github.com/user-attachments/assets/e5311212-d3cd-4d48-b0eb-9df967bc4e66" />

- Select "Client-1" in the home page of Azure.
- Select "Network" then "Network Setting" on the left hand column.
- Select the "Network Interface" name then "DNS Servers" under setting on the left hand column.
- Select custom then enter Private IP of "DC-1" and save it.
<img width="422" alt="DNS Setting Client-1" src="https://github.com/user-attachments/assets/7cc391ba-cc8a-449f-b298-1adb340d8776" />

**Step 3: Verify Connection**

- Copy IP address of "DC-1" on Azure and login to the virtual machine by remote desktop connection.
- On Windows search bar, enter "Run" and type wf.msc 
- On Windows Firewall, select "Windows Defender Firewall Properties" and under each tab turn off the firewall state.
<img width="539" alt="Firewall disable" src="https://github.com/user-attachments/assets/043b7047-d2c5-41de-905f-c9e4023df8ab" />

- Copy IP address of "Client-1" on Azure and login to the virtual machine by remote desktop connection.
- Open command prompt and ping "DC-1" private IP address.
<img width="343" alt="Client-1 verify connnection" src="https://github.com/user-attachments/assets/7b81e4b5-633c-44c4-b39f-751943f05d2b" />


**Step 4: Installed Active Directory**

- Open Server Manager.
- Under "Manage", select "Add Roles and Features".
- Follow the steps and add "Active Diretory Domain Services" role.
- Keep following th steps and select "Install" at the end.
<img width="429" alt="DA Installation" src="https://github.com/user-attachments/assets/59bbf30f-21da-4530-8900-50f46e6203f0" />

- Prompted for deployment configuration, select "Add a new forest", create a domain name (e.g. mydomain.net), and set password "Password1234".
- Continue with the configuration and restart DC-1.
<img width="480" alt="New forest Deployment" src="https://github.com/user-attachments/assets/fcf157e8-f77f-439c-aae4-8c11a6bb3a6f" />

- Log back in as "mydomain.net\Tech".



**Step 5: Setup User Accounts in Active Directory**

- Open "Active Direcoty Users and Computer" in DC-1.
- Create new Organiztional Unit call "_EMPLOYEES".
- Create a new employee name "Jane Doe" with user name "jane.doe" and a password "Password1234".
- Add "jane.doe" to the "Domain Admins" Security Group under "Member of" tab.
<img width="455" alt="Jane Doe creation" src="https://github.com/user-attachments/assets/664c0a66-5f7f-4b62-b927-d3c44eda901f" />

- Log out of DC-1 and sign in with "mydomain.net\jane.doe".

**Step 6: Add Client-1 to Domain**

- Open "Advance System Settings" in Client-1.
- Under tab "Computer Name", click "Change".
- Under "Member of" select "Domain" and type your domain name(mydomain.net).
- Join the domain(mydomain.net) and restart your computer.
<img width="544" alt="Client-1 joins domain" src="https://github.com/user-attachments/assets/0a7dbc37-82a7-4953-a7f0-85f346148a41" />

- Verify Client-1 is in domain by logging in DC-1, open Active Directory Users and Computer under "Computers".
<img width="363" alt="Client-1 joins computer file" src="https://github.com/user-attachments/assets/e8a46a57-05d4-451b-b72b-5cf526d418ff" />



**Step 7: Remote Access Domain Users**

- Login to "Client-1" as Jane Doe (mydomain.net\jane.doe)
- In Settings, go to "Remote Desktop Settings".
- Select "Select Users for Remote Access"
- Type "Domain Users" and select Check Names and apply the setting.
<img width="297" alt="Domain user remote access" src="https://github.com/user-attachments/assets/ac4557a5-1c8a-4ca8-b0fa-d6c2ac6da307" />


**Step 8: Create Users**

- Log in DC-1 as "mydomain.net\jane.doe".
- Open PowerShell_ise as an adminstrator.
- Run a script call Generate-Names-Create-Users.ps1 [(Josh Madokor's source code)](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
- Once finished, open Active Directory Users and Computer and observe new users under "_EMPLOYEES"
<img width="311" alt="User Account Added" src="https://github.com/user-attachments/assets/2c58ccc0-928a-4c04-ac63-f177c27b495b" />

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
