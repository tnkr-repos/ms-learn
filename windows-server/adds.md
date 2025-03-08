# ACTIVE DIRECTORY DOMAIN SERVICES

## INTRODUCTION TO AD DS

- As a Windows Server Administrator you are responsible for managing AD DS objects, such as users, groups and OUs

### DEFINE AD DS

- The AD DS database is the central store of all domain objects, such as users accounts, computer accounts, and groups. AD DS provides a searchable, hiearchical directory and a method for applying configuration and security settings for objects in an enterprise. It includes physical and logical components. In addition, you can use AD DS options to perform actions such as:
  - Installing, configuring and updating apps
  - Managing the security infrastructure
  - Enabling Remote Access Service and DirectAccess
  - Issuing and managing digital certificates

- AD DS logical components are structures that you use to implement an AD DS design that is appropriate for an organization
  - Partition - Also known as a naming context, it is a portion of the AD DS database. Although the database consists of one file named `Ntds.dit`, different partitions contain different data. For example, the schema partition contains a copy of the Active Directory schema and the domain partition contains the users, computers, groups, and other objects specific to the domain. Active Directory stores copies of partitions on multiple domain controllers and updates them through directory replication
  - Schema - The set of definitions of the object types and attributes that you use to define the objects created in AD DS.
  - Domain - A container for objects such as users and computers. A domain maps to a specific partition and you can organize the domain with parent-child relationships to other domains.
  - Domain Tree - A hierarchical collection of domains that share a common root domain and a contiguous Domain Name System (DNS) namespace.
  - Forest - A forest is a collection of one or more domains that have a common AD DS root, a common schema, and a common global catalog.
  - OU - A container object for users, groups, and computers that provides a framework for delegating administrative rights and administration by linking Group Policy Objects (GPOs).
  - Container - An object that provides an organizational framework for use in AD DS. You can use the default containers, or you can create custom containers. You can't link GPOs to containers

- Physical components in AD DS are those objects that are tangible, or that described tangible components in the real world
  - Domain controller - Ccontains a copy of the AD DS database. For most operations, each domain controller can process changes and replicate the changes to all the other domain controllers in the domain
  - Data store -  copy of the data store exists on each domain controller. The AD DS database uses Microsoft Jet database technology and stores the directory information in the Ntds.dit file and associated log files. The C:\Windows\NTDS folder stores these files by default.
  - Global catalog server - A global catalog server is a domain controller that hosts the global catalog, which is a partial, read-only copy of all the objects in a multiple-domain forest. A global catalog speeds up searches for objects that might be stored on domain controllers in a different domain in the forest.
  - Read-only Domain Controller - An RODC is a special read only installation of AD DS. RODCs are common in branch offices where physical security isn't optimal, IT support is less advanced than in the main corporate centers, or line-of-business applications need to run on a domain controller.
  - Site - A site is a container for AD DS objects, such as computers and services that are specific to a physical location. This is in comparison to a domain, which represents the logical structure of objects, such as users and groups, in addition to computers.
  - Subnet - A subnet is a portion of the network IP addresses of an organization assigned to computers in a site. A site can have more than one subnet.



### DEFINE USERS, GROUPS, AND COMPUTERS
### DEFINE AD DS FORESTS AND DOMAINS
### DEFINE OUS
### MANAGE OBJECTS AND THEIR PROPERTIES IN AD DS

## MANAGE AD DS DOMAIN CONTROLLERS AND FSMO ROLES

## IMPLEMENT GROUP POLICY OBJECTS

## MANAGE ADVANCED FEATURES OF AD DS

## IMPLEMENT AND MANAGE ACTIVE DIRECTORY CERTIFICATE SERVICES