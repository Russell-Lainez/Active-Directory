<p align="center">
<img src="https://www.31west.net/wp-content/uploads/2022/11/what-is-active-directory-and-why-is-it-used.png"/>
</p>

<h1>Deploying, Configuring, and Using Active Directory</h1>
In this demonstration, I will walk through how to deploy on-premises Active Directory using Azure Virtual Machines and how to utilize common Active Directory features. 



<h2>Environments, Applications, and Services Used </h2>

- Microsoft Azure (Virtual Machines/Virtual Network)
- Remote Desktop
- Windows Server 2022
- Windows 10 Pro (22H2)
- Active Directory Users and Computers



<h2>Demonstration</h2>

<h3>Virtual Machine Set Up and Active Directory Deployment</h3>
<p>
Step 1: I created 2 virtual machines (VM) and placed them in the same virtual network:

  - VM: dc-1 - running Windows Server which will act as the domain controller 

  - VM: client-1 - running Windows 10 Pro which will act as client computers 

  ![image](https://github.com/user-attachments/assets/6b5c03b8-27bb-444e-b1d3-3ab4923375cd)


Step 2: I changed dc-1’s NIC settings to have a static private IP address. The private IP address is set to dc-’s private IP address. 

For client-1’s DNS server setting, I changed it to dc-1’s private IP address. This way client-1 uses dc-1 to resolve names instead of Azure. dc-1 will still use Azure’s DNS server to resolve names. Additionally, this will help client-1 connect to the domain controller. 

![image](https://github.com/user-attachments/assets/b01818e8-9535-4c46-abc7-5d89fa1eb897)
![image](https://github.com/user-attachments/assets/f9422203-adc5-49ee-bf54-09eb0bb5b47c)

Step 3: After logging into dc-1, I opened Server Manager and installed Active Directory Domain Services Then, I proceeded to promote dc-1 to be an actual domain controller and set the domain name to be mydomain.com. 

![image](https://github.com/user-attachments/assets/183a5d25-2c82-4fd2-9b01-1708ad7de213)
![image](https://github.com/user-attachments/assets/84ac92d2-fba2-4677-bbc8-2fc174f22bcd)
![image](https://github.com/user-attachments/assets/7021709b-c94d-4e46-abbb-b38fc4ff8a0c)

Step 4: After installation, dc-1 restarted itself to be the domain controller. In order to log back in, I had to specify myself as a domain user by using mydomain.com. 

![image](https://github.com/user-attachments/assets/cf5a437c-d74e-4da7-8498-b4dfca5dcfb9)

  
<h3>Group Policy Object and Organizational Units</h3>

I then opened Active Directory Users and Computers to create a domain admin user with which to login as and utilize for upcoming actions. I created an admin user, Jane Doe, and proceeded to add her to the Domain Admins security group. 

![image](https://github.com/user-attachments/assets/9abaa6a2-f0e3-470d-a2af-99d7e73f70d8)

I then created two organizational units to track and organize users appropriately:  _EMPLOYEES (which will be used shortly) and _ADMINS (where I placed Jane Doe).

![image](https://github.com/user-attachments/assets/e6c61fe6-5842-4a90-9af6-ea1df3b60b38)

<h3>Joining client-1 to the domain</h3>

I logged into client-1 as Jane Doe then proceeded to add client-1 by going to System < Rename the PC < Computer name < Change < Member of domain: mydomain.com. I then verified client-1 was added to the domain and created a new organization unit called _CLIENTS to track and organize all client computers. 

![image](https://github.com/user-attachments/assets/bc5ea614-54da-4af6-8bbf-70885673a770)

![image](https://github.com/user-attachments/assets/90e83f2f-edcc-442f-816c-06fbb5c719d1)


<h3>Using Active Directory</h3> 

<h4>Creating users and allowing them to log into client-1</h4>

After logging into client-1 as Jane Doe, I ensured all domain users/any client can use the client computer. 

![image](https://github.com/user-attachments/assets/405ae9c6-5183-4c3a-acc9-9c6f05a0dc1e)

I created fake user profiles and had them filed in the _EMPLOYEES organization unit. Now, any one of these employees would be able to log into client-1/the one client computer we have registered in our domain. 

![image](https://github.com/user-attachments/assets/b87e7dbf-fe01-4a21-845b-c37e1a5edd52)

![image](https://github.com/user-attachments/assets/30c0462d-c3f3-4a46-8c3d-47c549ab4e4e)

<h4>Incorrect Password Lockouts</h4>

In dc-1, I opened Group Policy Management Console to configure the domain policy on incorrect passwords and account lockouts. 

![image](https://github.com/user-attachments/assets/3a787ef2-c450-49e0-a594-33ca81986950)

Back in client-1, as Jane Doe, I forced the computer to accept the new policy, by opening PowerShell as an administrator, and typing into the command line, gpupdate /force.

![image](https://github.com/user-attachments/assets/6efcb9ac-c10f-4e5c-80a5-e0ed032b3a62)

After the restarting the client, I attempted to login as one of the created fake employees until I received the account lock message.

I went into dc-1 as Jane Doe and proceeded to find the user in Active Direcory Users and Computers and selected to unlock their account.

![image](https://github.com/user-attachments/assets/171c88e2-7378-4398-8dfc-171095547459)

<h4>Enabling and Disabling Accounts</h4>

After logging into dc-1 as Jane Doe, I went into Active Directory Users and Computers, selected a random employee, and disabled their account. 

![image](https://github.com/user-attachments/assets/ffe2e8ef-3680-4dc5-ade7-e33168509df6)

I logged out of dc-1 and attempted to log into client-1 as the employee. I verfied the account was disabled. 

![image](https://github.com/user-attachments/assets/427239ca-9d09-49d8-a935-298456d928e0)

To enable the account again, I logged back into dc-1 as Jane Doe and selected to enable.

![image](https://github.com/user-attachments/assets/ad9306a7-0a6e-44b2-8591-cde86fe2ce53)
![image](https://github.com/user-attachments/assets/cd3e6786-50ca-4513-98e3-8406425f8983)

</p>
<br />
