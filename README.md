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


![Screenshot 2024-03-12 043057 - Copy](https://github.com/hectorvalencia2/configure-ad/assets/161524174/04ab3c4a-3dd5-4b15-81d2-bc1ae99b52be)

- Setup Resources in Azure
Create the Domain Controller VM (Windows Server 2022) named “DC-1”. Take note of the Resource Group and Virtual Network (Vnet) that get created at this time. Set Domain Controller’s NIC Private IP address to be static. Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a. Ensure that both VMs are in the same Vnet

![Screenshot 2024-02-28 141412](https://github.com/hectorvalencia2/configure-ad/assets/161524174/07ab50a3-6b32-4f5e-ba21-aa15662515d7)
![Screenshot 2024-02-28 142227](https://github.com/hectorvalencia2/configure-ad/assets/161524174/ea20b3e5-113b-4994-9df8-939503dbaa74)

- Using remonte desktp login to DC-1 and install Active Directory Domain Services. Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is). Restart and then log back into DC-1 as user: mydomain.com\labuser

![Screenshot 2024-02-28 145606](https://github.com/hectorvalencia2/configure-ad/assets/161524174/cebd96a6-7125-429b-a4e7-5cf7fe665199)

- In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”. Create a new OU named “_ADMINS”. Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”. Add jane_admin to the “Domain Admins” Security Group. Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”. User jane_admin as your admin account from now on

![Screenshot 2024-02-28 150602](https://github.com/hectorvalencia2/configure-ad/assets/161524174/260f4d18-172f-4a5d-b5b8-255155213f7c)
![Screenshot 2024-02-28 150751](https://github.com/hectorvalencia2/configure-ad/assets/161524174/c85837d0-090f-4dcc-ab22-de7672cd3383)

- Get DC-1 Private IP Address from the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address. From the Azure Portal, restart Client-1. Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart). Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain

![Screenshot 2024-03-12 042324](https://github.com/hectorvalencia2/configure-ad/assets/161524174/3318332c-d879-49a1-9719-7f200a9b3d2a)

- Login to DC-1 as jane_admin. Open PowerShell_ise as an administrator. Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1). Run the script and observe the accounts being created. When finished, open ADUC and observe the accounts in the appropriate OU. Attempt to log into Client-1 with one of the accounts



