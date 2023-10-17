![image](https://github.com/itnatepena/configure-ad/assets/147539410/fdd80f20-2b7b-4840-9563-3f091bb7870c)

<h1>Configuring Active Directory in Azure</h1>
This guide is a follow-up to the previous lab where I installed Active Directory and established a domain controller. In this lab, I will configure Active Directory and demonstrate how to add a client to the domain while creating user accounts.

## Project Summary

This lab guide walks you through the process of configuring Active Directory in an Azure environment. It covers the following key aspects:

- **Environments Used:** Azure (Virtual Machines/Compute).
- **Technology/Applications/Services Used:** Microsoft Azure, Remote Desktop, Active Directory Domain Services, and PowerShell for account creation.

## Technologies Used

- **Operating Systems:** Windows Server 2022, Windows 10 Pro (21H2).

<h2>Configuration Steps</h2>

<p>
Now that Active Directory is installed on the domain controller VM, it's time to set up Organizational Units (OUs) and User accounts. Using the Active Directory Users and Computers console, follow these steps:
</p>

<p>
1. Right-click on the domain you created (e.g., natepena.com) and create new Organizational Units (OUs). I've created two OUs, "_EMPLOYEES" and "_ADMINS."
  
![image](https://github.com/itnatepena/configure-ad/assets/147539410/8b2567a2-3ac9-4d2f-8be9-cbfffd089634)

2. Within the "_ADMINS" OU, I created a User named Jane Doe. Jane's account will be given administrative privileges through the use of a Security Group. To grant admin privileges, right-click on the user, open their Properties, and click "Member Of" to add the appropriate security group. 
![image](https://github.com/itnatepena/configure-ad/assets/147539410/a1cef736-9436-4717-9900-e584f80bc169)

In my case, I added Jane to the Domain Admins security group. From now on, I will be using Jane's account for further changes. I'll log in as janedoe@natepena.com who now has full access.
</p>

<p>
Before the client can join the domain, it's essential to configure the DNS settings. The DNS server must point to the domain controller's private IP address. On the Azure portal:
</p>

<p>
1. Open the Networking tab and click on Network Interface.
2. In the DNS servers, enter the domain controller's private IP address and save the changes.
  
![image](https://github.com/itnatepena/configure-ad/assets/147539410/e9e01aa3-4323-442e-b956-413cba0bddf9)

3. Restart the client VM to ensure the DNS changes are saved.
</p>

<p>
Now, it's time to make the client VM join the domain:
</p>

![image](https://github.com/itnatepena/configure-ad/assets/147539410/05c3d8b6-2e5c-45ed-8ffe-fb669a531ead)
<p>Here I am quickly checking connectivity</p>

<p>
1. In the System menu of the client VM, click on "Rename this PC (advanced)" and then "Change."
2. Enter the domain and necessary credentials to let the client join the domain. 
  
![image](https://github.com/itnatepena/configure-ad/assets/147539410/0f6fb398-2a63-48fd-b5e2-16394b2db824)

![image](https://github.com/itnatepena/configure-ad/assets/147539410/4ee836bd-d206-4927-8529-9c6ea7de85a5)


I am logging in as Jane Doe for the lab. The login credentials must be input within the context of the domain path. The client should now be part of the domain, and it will appear in the Computers section on the domain controller.
</p>

<p>
Before users in the domain can use the client computer, Remote Desktop has to be enabled for non-administrative users:
</p>

<p>
  
![image](https://github.com/itnatepena/configure-ad/assets/147539410/eb71dbe0-cd0a-406c-9249-160e409de35e)

1. While logged in as the administrator (Jane in my case), open System Properties.
2. Click on Remote Desktop and "Select users that can remotely access this PC."
3. Allow Domain Users access to Remote Desktop.
![image](https://github.com/itnatepena/configure-ad/assets/147539410/47e8cf50-d240-45fa-9d38-f17c29a433c4)

 Non-administrative users can now log in to Client-1. Although a Group Policy can achieve the same result for multiple systems, I won't be using it for this lab.
</p>

<p>
Creating users can be done manually or through the use of a script. For this lab, I'll be using a PowerShell script, which can be found <a href="https://github.com/AsiaPonder001/BunchofUsers/blob/main/README.md?plain=1">here!</a>
</p>

<p>
On the domain controller, open PowerShell ISE as an administrator, and make sure you're logged in with admin credentials.
  
![image](https://github.com/itnatepena/configure-ad/assets/147539410/e3f923fb-42c0-4746-9b51-3c3f346f2b0c)

 Create a new file, paste the script into the ISE console, and run the script to observe the accounts being created.

![image](https://github.com/itnatepena/configure-ad/assets/147539410/9d6b076f-ac4c-4797-8ce0-494eedb0f60a)

</p>

<p>
After creating the users, Client-1 can now be signed in as one of the new users created by the PowerShell script. Choose a name and sign in with the context of the domain. In my case, it's natepena.com\git.gud.

![image](https://github.com/itnatepena/configure-ad/assets/147539410/f44be764-828b-4ab7-ad1f-eee1a71cc7a2)

</p>

<h2>Lessons Learned</h2>

This lab provided valuable experience in setting up Active Directory and joining clients to the domain. I also created user accounts and assigned necessary permissions. Active Directory, despite the menu navigation, isn't difficult to learn. This lab serves as a stepping stone for exploring DNS settings and file permissions in more depth in future labs.
