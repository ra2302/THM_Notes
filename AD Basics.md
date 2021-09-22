**Physical AD** is usually an On-Premise infrastructure with all the hardware and stuff, These can include anything from Domain Controllers to all the other kinds of servers discussed earlier in the forest

**Domain Controllers** -
  It's a windows server which hosts the Active Directory Domain Services and has been promoted to the epic title of domain Controller in the forest
  It does the following things -
    Holds basic AD data
    Handles authentication and authorization Services
    Replicate updates from other domain controllers in the forest
    Allows the admins to access and manage domain resources

**AD DS(Active Directory Domain Services) Data Store** -
  AA DS stores the databases and the processes needed to store and manage directory information such as users, groups and services.
    Contains NTDS.dit - which is a database file which contains all the information of AD doamin controller as well as password hashes for domain users
    Stored by default in %SystemRoot%\NTDS
    Accessible only by the domain controller

**NOW WHAT IS THE FOREST?**
The forest is what defines everything. It can be called a container which holds all the bits and the pieces of the network together. Without all the other domains and the trees would not be able to interact
When we say "forest" it is only a way of describing the connection created between these trees and domains by the network.
**Overview -**
  Collection of one or more domain trees inside of an active directory network. It is what categorizes the parts of network as a whole
  It consists of these parts -
    Trees - A hierarchy of domains in the AD DS
    Domains - Used to group and manage objects
    Organization Units(OUs) - Containers for groups, computers, users, printers and other OUs
    Trusts - Allows user to access the resource in other Domains
    Objects - Groups, computers, users, printers, shares
    Domain Services - DNS server, LLMNR, IPv6
    Domain Schema - Rules for Object creation

**USERS + GROUPS**
When an domain controller is created, by default there are two users, Administrator and Guest. Rest all the users are upto us to create

**User overview** -
  They are the core of the AD. There 4 basic types of users we can expect to see in the AD network. Also not to mention, an Organization can have different as well depending on their way of managing user permissions
    Domain Admins - Big bois, they rule the domains and are the only ones which can access the domain controllers
    Service Accounts - These are rarely used only in the case of service maintenance. They are needed by windows for services such as SQL to pair a service with a service Account
    Local Admins - They can make changes to their local machines as admins and may even control other normal users, but they can't access the domain controller
    Domain Users - The most basic everyday users. Log in to the machines. have authorization access and may even have local admin rights.

**Groups** - They are basically used to provide the users with the permissions to the objects and organizing them in to groups with specific groups. There are two overarching types of AD groups -
  Sec Groups - The main groups which specify the permissions for the users
  Distribution groups - They are the groups which are used to specify email distribution lists.

  Default Security Groups - There are hell lot of default groups. they are -
    Domain Controllers - All the big bois in the domain
    Domain guests - As the name says
    Domain Users - As the name says
    Domain Computers - Workstations and servers in the domain
    Domain Admins - Administrators of the domain
    Enterprise Admins - Administrators of the Enterprise
    Schema Admins - Designated administrators of the schema
    DNS Admins - DNS Administrators Group
    DNS Update Proxy - The DNS clients which are allowed to perform dynamic updates on the behalf of some other clients
    Allowed RODC Password Replication Group - Members in this group can have their passwords replicated to all read-only domain controllers in the domain
    Group Policy Creator Owners - Members in this group can modify group policy for the domain
    Denied RODC Password Replication Group - Members in this group cannot have their passwords replicated to any read-only domain controllers in the domain
    Protected Users - Members of this group are afforded additional protections against authentication security threats. See http://go.microsoft.com/fwlink/?LinkId=298939 for more information.
    Cert Publishers - Members of this group are permitted to publish certificates to the directory
    Read-Only Domain Controllers - Members of this group are Read-Only Domain Controllers in the domain
    Enterprise Read-Only Domain Controllers - Members of this group are Read-Only Domain Controllers in the enterprise
    Key Admins - Members of this group can perform administrative actions on key objects within the domain.
    Enterprise Key Admins - Members of this group can perform administrative actions on key objects within the forest.
    Cloneable Domain Controllers - Members of this group that are domain controllers may be cloned.
    RAS and IAS Servers - Servers in this group can access remote access properties of users

**TRUST + POLICIES**
They both really go hand in hand to help domain and trees communicate with each other and maintain security inside of the network. They put the rules in place for the conduct of domains and their interactions with each other in the forest.

  Domain Trust Overview -
    Trusts are used by users to gain access to other resources. They outline how one is supposed to communicate inside the forest. Two types of trust -
      Directional - Direction flows from trusting domain to a trusted domain
      Transitive - The relationship expands beyond just two domains to include other trusted domains
  Domain Policies review -
    Policies are the major part of the AD environment. They dictate how the server behaves and which rules it will and will not follow. Policies usually apply to the whole domain. Basically a rulebook. Domain admin can modify this rulebook.

**DOMAIN SERVICES + AUTHENTICATION**
  Domain services -  The services which the domain controller provides to the rest of the doamin/tree. Some default services -
    LDAP - Lightweight directory authentication protocol, provides authentication and stuff
    Certificate Services - Allows creation/validation/revoke of public key certificates
    DNS,LLMNR, NBT-NS - Used for identifying IP hostnames
  Domain authentication - Two main types of authentication -
    NTLM - Default Windows authentication services protocol, uses an encrypted challenge/response protocol
    Kerberos - Default authentication service for Active Directory. Uses ticket-granting tickets and service tickets to authenticate users and give users access to others resources/

AD IN THE CLOUD
  Usually hosted as Azure AD, default settings are pretty pog and secure as compared to the on-premise one.
  Azure acts as middle man between your physical AD and the user's sign in. It allows more secure transactions.
  Security Overview - As compared to On premise this is what Azure AD works
     On premise AD
      * LDAP  
      * NTLM
      * Kerberos
      * OU Tree
      * Domains and Forest
      * Trusts
    Azure AD
      Rest APIs
      OAuth/SAML
      OpenID
      Flat Structure
      Tenants
      Guests

Hands on Lab -
  Imported the PowerView Module in to Powershell -> . .\PowerView.ps1
    PowerView Cheatsheet - https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993

  Task 1 -> What is the name of the Windows 10 operating system?
            Get-NetComputer -fulldata | select operatingsystem
  Task 2 -> What is the second "Admin" name?
            Get-NetUser | select givenname
  Task 3 -> Which group has a capital "V" in the group name?
            Get-ADGroup -Filter * | select name OR Get-NetGroup -Groupname *
  Task 4 -> When was the password last set for the SQLService user?
            Get-NetUser | select name,pwdlastset OR Get-NetUser -SPN | ?{$_.memberof -match 'Domain Admins'}
