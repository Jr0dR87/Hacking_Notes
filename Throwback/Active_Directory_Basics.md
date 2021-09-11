### What is Active Directory? 
Active Directory is a collection of servers inside a domain that are collective parts of a bigger forest of domains that make up the AD Network.
Important pieces of AD are 
- Domain Controllers (The head Honcho in AD)
- Forest, Trees, Domains
- Users + Groups
- Trust
- Policies
- Domain Services

#### What is a Domain Controller?
A Domain Controller is a Windows Server that has AD DS installed (Active Directory Domain Services) and has been promoted to a domain controller in the forest.
Domain Controllers are the center of AD.

- holds the AD DS data store 
- handles authentication and authorization services 
- replicate updates from other domain controllers in the forest
- Allows admin access to manage domain resources

#### AD DS Data Store

The Active Directory Data Store holds the databases and processes needed to store and manage directory information such as users, groups, and services.
List of contents of AD DS Data Store
- Contains the NTDS.dit (a database that contains all of the information of an Active Directory domain controller as well as password hashes for domain users)
- Accessible only by the domain controller

#### Forest 

A forest is a collection of one or more domain trees inside of an Active Directory network.
- Trees - A hierarchy of domains in Active Directory Domain Services
- Domains - Used to group and manage objects 
- Organizational Units (OUs) - Containers for groups, computers, users, printers and other OUs
- Trusts - Allows users to access resources in other domains
- Objects - users, groups, printers, computers, shares
- Domain Services - DNS Server, LLMNR, IPv6
- Domain Schema - Rules for object creation

#### Users
- Domain Admins - This is the big boss: they control the domains and are the only ones with access to the domain controller.
- Service Accounts (Can be Domain Admins) - These are for the most part never used except for service maintenance, they are required by Windows for services such as SQL to pair a service with a service account
- Local Administrators - These users can make changes to local machines as an administrator and may even be able to control other normal users, but they cannot access the domain controller
- Domain Users - These are your everyday users. They can log in on the machines they have the authorization to access and may have local administrator rights to machines depending on the organization.

#### Domain Policies
Policies are rules that are followed by AD Objects to help operations run smoothly. They apply to the domain as a whole.


#### Domain Services 
- LDAP - Lightweight Directory Access Protocol; provides communication between applications and directory services
- Certificate Services - allows the domain controller to create, validate, and revoke public key certificates
- DNS, LLMNR, NBT-NS - Domain Name Services for identifying IP hostnames
