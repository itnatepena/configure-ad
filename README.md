<h1>Configuring Active Directory in Azure</h1>
This guide is a follow-up to the previous lab where I installed Active Directory and established a domain controller. In this lab, I will configure Active Directory and demonstrate how to add a client to the domain while creating user accounts.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used</h2>

- Windows Server 2022
- Windows 10 Pro (21H2)

<h2>Configuration Steps</h2>

<p>
Now that Active Directory is installed on the domain controller VM, it's time to set up Organizational Units (OUs) and User accounts. Using the Active Directory Users and Computers console, follow these steps:
</p>

<p>
1. Right-click on the domain you created (e.g., ernestotest.com) and create new Organizational Units (OUs). I've created two OUs, "_EMPLOYEES" and "_ADMINS."
2. Within the "_ADMINS" OU, I created a User named Jane Doe. Jane's account will be given administrative privileges through the use of a Security Group. To grant admin privileges, right-click on the user, open their Properties, and click "Member Of" to add the appropriate security group. In my case, I added Jane to the Domain Admins security group. From now on, I will be using Jane's account for further changes. I'll log in as Jane_Admin.
</p>

<p>
Before the client can join the domain, it's essential to configure the DNS settings. The DNS server must point to the domain controller's private IP address. On the Azure portal:
</p>

<p>
1. Open the Networking tab and click on Network Interface.
2. In the DNS servers, enter the domain controller's private IP address and save the changes.
3. Restart the client VM to ensure the DNS changes are saved.
</p>

<p>
Now, it's time to make the client VM join the domain:
</p>

<p>
1. In the System menu of the client VM, click on "Rename this PC (advanced)" and then "Change."
2. Enter the domain and necessary credentials to let the client join the domain. I am logging in as Jane Doe for the lab. The login credentials must be input within the context of the domain path. The client should now be part of the domain, and it will appear in the Computers section on the domain controller.
</p>

<p>
Before users in the domain can use the client computer, Remote Desktop has to be enabled for non-administrative users:
</p>

<p>
1. While logged in as the administrator (Jane in my case), open System Properties.
2. Click on Remote Desktop and "Select users that can remotely access this PC."
3. Allow Domain Users access to Remote Desktop. Non-administrative users can now log in to Client-1. Although a Group Policy can achieve the same result for multiple systems, I won't be using it for this lab.
</p>

<p>
Creating users can be done manually or through the use of a script. For this lab, I'll be using a PowerShell script, which can be found <a href="https://github.com/AsiaPonder001/BunchofUsers/blob/main/README.md?plain=1">here!</a>
</p>

<p>
On the domain controller, open PowerShell ISE as an administrator, and make sure you're logged in with admin credentials. Create a new file, paste the script into the ISE console, and run the script to observe the accounts being created.
</p>

<p>
After creating the users, Client-1 can now be signed in as one of the new users created by the PowerShell script. Choose a name and sign in with the context of the domain. In my case, it's ernestotest.com\bon.rovej.
</p>

<h2>Lessons Learned</h2>

This lab provided valuable experience in setting up Active Directory and joining clients to the domain. I also created user accounts and assigned necessary permissions. Active Directory, despite the menu navigation, isn't difficult to learn. This lab serves as a stepping stone for exploring DNS settings and file permissions in more depth in future labs.
