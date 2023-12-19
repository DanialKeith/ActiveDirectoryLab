# VirtualBox Active Directory

![Active Directory(2)](https://github.com/DanialKeith/ActiveDirectoryLab/assets/154257195/e31f4fe8-9579-4375-8337-2c9d3a38a1cc)


## Overview
To better understand Active Directory and how Windows networks operate, I conducted this lab to mimic a real-world Active Directory setup. My goal was to learn more about Active Directory and explore Windows network systems. Using Oracle's VirtualBox, I created a simulated network with two virtual machines: one with Windows Server 2019 acting as the Domain Controller, and another running Windows 10, acting as a network client.

## Objectives
### The lab's journey will include:
-	Initiating Active Directory Domain Services from the ground up.
-	Establishing a dedicated Domain Admin account to maintain security and oversight.
-	Implementing a DHCP server to assign IP addresses automatically.
-	Illustrating the setup of an Organizational Unit to streamline user management.
-	Investigating Routing and Remote Access services to mirror an enterprise intranet.
-	Tailoring the Network Interface Card (NIC) settings to guarantee both local and internet connections.
-	Using PowerShell to efficiently create a multitude of user accounts.
-	Engaging with the client machine to model a typical user sign-in using one of the newly crafted accounts.
  
## Technologies Used:
-	Oracle VirtualBox
-	Windows Server 2019 and Windows 10 ISO files
-	PowerShell
  
## Installation
#### Setting Up Virtual Machines in VirtualBox
-	Launch Oracle VirtualBox and initiate a new virtual machine setup.
-	Label the first virtual machine "DC," choose Microsoft Windows as the operating system, and specify Windows (64-bit) as the version.
-	Allocate a minimum of 4 GB of RAM and create a virtual hard disk with at least 50 GB.
-	Duplicate this procedure for the Windows 10 machine, naming it "CLIENT."
### Operating Systems Installation and Preliminary Configuration
I began by linking the ISO files to their respective virtual machines. The Windows Server 2019 installation demanded particular attention to ensure the administrative access was configured correctly.
-	**Installation of Windows Server 2019**: Carry out the guided setup until you reach the point of account configuration.
  
![1](https://github.com/DanialKeith/ActiveDirectoryLab/assets/154257195/71f18d72-e148-49b8-931f-865c5a62126a)


-	**Admin Account Creation**: During setup, you'll be instructed to create an admin account. Here, I designated a username and established a secure, unique password, considering the elevated access of this account.
  
<p align="center">
  <img src="https://github.com/DanialKeith/ActiveDirectoryLab/assets/154257195/993e9110-db31-4a8c-bef8-5570c270cae5">
</p>


![3](https://github.com/DanialKeith/ActiveDirectoryLab/assets/154257195/006cfc2f-c6d0-44d6-a397-907f584dbd85)



### Operating Systems and NIC Configuration
Post ISO file attachment, I proceeded with the installation, which led to the successful setup of both operating systems.

Moreover, for the Domain Controller, setting up dual Network Interface Cards (NICs) was crucial for managing network operations:

-	**NIC for Broad Internet Access**: The first NIC was set to Network Address Translation (NAT) mode to provide internet access.
-	**NIC for the Internal Network**: The second NIC was assigned to our private virtual network to facilitate internal communication.
  
Such a configuration of dual NICs provides the necessary network flexibility and is in line with the lab's goals.

## Active Directory Domain Services Integration

-	Upon starting Windows Server, I entered the Server Manager and added the "Active Directory Domain Services" role diligently.
  
![4](https://github.com/DanialKeith/ActiveDirectoryLab/assets/154257195/78e5a10e-89b7-4612-9447-c8b98f85cb1c)


## Creating a Domain Admin Account
-	Access the Active Directory Users and Computers tool.
-	Create a new user account by selecting New > User and define your Domain Admin account with a secure password.
## DHCP Server Configuration
-	Utilizing the Server Manager again, I integrated the DHCP Server role to dynamically distribute IP addresses to client machines.
## Organizational Unit Structure
-	In the Active Directory Users and Computers interface, establish a new Organizational Unit by right-clicking the domain and selecting New > Organizational Unit.
## Routing and Remote Access Setup
-	Add the Routing role via Server Manager.
-	Configure the service to establish an internal network structure.
## NIC Configuration for Internet Connectivity
-	Open the Network and Sharing Center and adjust the adapter settings.
-	Set up the NIC to establish an internet connection.
## PowerShell for Bulk User Addition
In organizations with a large number of users, manually adding each user is inefficient. To streamline this process, I use a PowerShell script for batch user addition. We'll explore the script ```BulkAddUserScript.ps1``` to understand its components and functionality:
### Setting Up Variables:
-	```$PASSWORD_FOR_USERS = "Password1"``` : Assigns a default password, "Password1", to all users. This is a temporary setup suitable for a lab environment, not recommended for real-world applications.
-	```$USER_FIRST_LAST_LIST = Get-Content .\names.txt``` : Reads a file ```names.txt``` containing a list of names, each formatted as "FirstName LastName".
### Converting Password to Secure Format:
-	```ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force``` : Transforms the plaintext password into a secure string for enhanced security in user management tasks.
### Creating an Organizational Unit (OU):
-	```New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false``` : Generates an OU named ```_USERS```, with no accidental deletion protection. Suitable for lab settings but should be reconsidered for actual use.
### Processing User Names:
    foreach ($n in $USER_FIRST_LAST_LIST) {
    ...
    }
-	The script loops through each name in the ```$USER_FIRST_LAST_LIST```.

### Extract the first and last names:
    $first = $n.Split(" ")[0].ToLower()
    $last = $n.Split(" ")[1].ToLower()

-	It splits each name into first and last names and converts them to lowercase.

### Construct the username:
    $username = "$($first.Substring(0,1))$($last)".ToLower()
- This line constructs a username by taking the first letter of the first name and appending the full last name, all in lowercase.
### Display progress in the console:
    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
-	Progress is displayed in the console during user creation.

### Create the new user:
-	```New-AdUser``` : This cmdlet is utilized to create a new user in Active Directory, placing the user in the ```_USERS``` OU. The password is set to never expire, which, while appropriate for a lab, should be set to expire in real-world applications for better security.
  
This automation highlights the efficiency of PowerShell in managing a Windows-based environment, showcasing its potential in real-world scenarios.

![6](https://github.com/DanialKeith/ActiveDirectoryLab/assets/154257195/f60bd4fb-f14e-4894-9e69-807ac56908fc)


### Client Machine Interaction
With the Windows 10 VM configured, I connected it to the server setup and experienced the login process with a user created through our automation script.

![5](https://github.com/DanialKeith/ActiveDirectoryLab/assets/154257195/b2445bd0-977d-4eca-b5ea-6d654ab447d0)

## Final Thoughts
The lab provided a practical experience in setting up and managing an Active Directory environment, demonstrating the use of PowerShell for automation and user management. This exercise offered insights into Windows networking, emphasizing the importance of automation in IT. The skills gained are relevant for real-world IT infrastructures, and future explorations could include Group Policies and cloud integration. Overall, the lab serves as a fundamental introduction to essential IT skills and concepts for both aspiring and current IT professionals.
