# WINDOWS SERVER FILE SERVERS AND STORAGE MANAGEMENT

- Objective:
	- Implement Windows Server file servers and storage
	- Implement Storage Spaces, data de-duplication, and Windows Server Storage Replica

## MANAGE WINDOWS SERVER FILE SERVERS

### INTRODUCTION

- As a Windows Server Administrator you are responsible for identifying the optimal configuration of Windows Server-based file servers. This involves choosing and applying the optimal file system depending on the intended use case, configuration of quotas and file screen on server volumes, and securing and optimizing protocols used to access file servers

### DEFINE THE WINDOWS SERVER FILE SYSTEM

- Before you can store data on a volume, you must first format the volume. To do so, you must select the file system that the volume should use

- A file system provides a range of features that implement storage and retrieval of files on storage devices. It allows you to organize files in a hierarchical structure and controls their format and naming convention. File systems support a wide range of storage devices, including hard disks and removable media. All the file systems available on Windows operating system consist of the following storage components:
	- Files - logical grouping of related data
	- Directories - hierarchical collection of directories and files
	- Volumes - collection of directories and files

- The Windows Server file system types include:
	- File Allocation Table (FAT), FAT32, and extended file allocation table (exFAT)
		- Simplest file system available in the Windows operating systems support
		- Keeps track of file system objects by using volume-level table
		- FAT maintains 2 copies of the table for resiliency. Both tables and the root directory must reside at a fixed location on the formatted disk
		- Cannot be used to create volumes larger than 4GB due to size limitations of the file allocation table. To accomodate larger disks, FAT32 was developed to support partitions of upto 64GB
		- exFAT is the file system designed for flash drives, with support for volume sizes larger than those available with FAT32. It works with media devices, such as modern flat panel TVs, media centers, and portable media players
		- Neither FAT nor FAT32 provide file system-level security. You shouldn't create FAT or FAT32-volumes on disks attached to the servers running any of the Windows Server operating systems. However you might consider using any of these to format external media such as USB flash drives
		- FAT and FAT32 support encryption through Encrypting File System (EFS)
	- The NT File System (NTFS)
		- Most common choice of the file system for the Windows Server operating system
		- Offers numerous improvements over FAT, which leverage advanced data structures to improve performance, reliability, and disk space utilization
		- Built-in security, with such access control capabilities as Access Control Lists (ACLs), auditing, file-system journaling, and encryption
		- Supports file system compression and encryption (but mutually exclusively and cannot be applied on the same file or folder)
		- Required when implementing volumes on servers hosting several Windows Server roles and features such as Active Directory Domain Services (ADDS), VSS, and the Distributed File System (DFS)
	- Resilient File System (ReFS)
		- Enhanced resiliency to data corruption through a more accurate detection mechanism and ability to remediate integrity issues online
		- Supports larger sizes for individual files and volumes, including their deduplication
		- Optimal file system choice for data volumes in Windows Server and boot volumes as well
		- Supports deduplication, but doesn't support file-level compression and encryption
		- Not suitable for removable media
	
- A sector is a minimum amount of data that can be read or written to a hard drive. They are usually fixed at 512bytes. Modern drives support larger sizes. Formatting a volume with a file system combines sectors into logical clusters, also referred to as allocation units (if sectors of a hard drive are 512bytes, a 4KB cluster will have 8 sectors). If you initiate the formatting process, you have the option of designating the preferred cluster size (or rely on defaults, which determine the cluster size based on the size of the volume)
- Cluster size represents the smallest amount of disk space that can be used to hold a file, based on the format defined by the file system. When file size doesn't match the individual or multiple cluster sizes, this results in some degree of disk space usage inefficiencies. But choosing a smaller cluster size could negatively impact performance, because reading or writing to a file might require an increased number of disk operations. You should optimise cluster size, as well as ensure that the cluster boundaries align with the underlying sectors
- To improve performance try to match the allocation unit size as closely as possible to the typical file or record size written to or read from the disk. For example, if you have a database that writes 8,192-byte records, the optimum allocation unit size would be 8 KB. This setting would allow the operating system to write a complete record in a single allocation unit on the volume. By using a 4-KB allocation unit size, the operating system would have to split the record across two allocation units and manage updates to the underlying metadata. By using an appropriately sized allocation unit, you can reduce the workload on the server's disk subsystem.

### LIST THE BENEFITS AND USES OF FILE SERVER RESOURCE MANAGER (FSRM)

- Use FSRM to manage and classify data that is stored on file servers. It includes features like:
	- Quota management - This feature facilitates limiting the space allowed for a volume or folder. Quotas can apply automatically to new folders that you create on a volume. You can also define quota templates that you can apply to new volumes or folders.
	- File screening management - This feature helps control the types of files that users can store on a file server. You can limit the file types with specific file extensions that users can store on your file shares. For example, you can create a file screen that doesn't allow users to save files with an.mp3 extension in a file server's personal shared folders. (File screening only considers the file extension. It doesn't examine the file contents)
	- Storage reports - This feature helps with identifying trends in disk usage and effectiveness of data classification. You can also monitor attempts by a selected group of users to save unauthorized files.
	- File Classification Infrastructure - This feature automates the data classification process. You can dynamically apply access policies to files based on their classification. Example policies include Dynamic Access Control for restricting access to files, file encryption, and file expiration. You can classify files automatically by using file classification rules, or you can classify them manually by modifying the properties of a selected file or folder.
	- File management tasks. This feature allows you to apply conditional policies and actions to files based on such criteria as file location, classification properties, in addition to file creation, modification, or access date. The actions that file management tasks support include the ability to expire files, encrypt files, or run a custom command.
	- Access-denied assistance. This feature generates custom error messages to users who aren't able to access files because of insufficient permissions or FSRM-based protection mechanisms.

- You can configure and manage FSRM by using the File Server Resource Manager Microsoft Management Console (MMC) console or by using Windows PowerShell.

- Features to be installed - File Server Resource Manager, and File Server Resource Manager Remote Server Administration Tools
- Check Firewall settings to verify if they allow Remote File Server Management incoming requests on the File Server
- Open `Windows Administrative Tools` -> `File Server Resource Manager` -> Right click and connect to another computer -> Select the File Server name from AD -> Configure `Quota Management` and `File Screening`

### DEFINE SMB AND ITS SECURITY CONSIDERATIONS

- Server Message Block (SMB) servers as the basis for Windows file sharing
- SMB is a TCP/IP-based network file sharing protocol that allows applications on a computer to read and write to files, and to request services from server programs in a computer network. Using the SMB protocol, an application (or the user of an application) can access files or other resources at a remote server. This allows applications to read, create, and update files on the remote server
- Benefits of SMB 3?
	- SMB 3.0, which Microsoft introduced in Windows Server 2012, includes the following features:
	- SMB Transparent Failover. This feature enables you to perform the hardware or software maintenance of nodes in a clustered file server without interrupting server applications that are storing data on file shares.
	- SMB Scale Out. In clustered configurations, you can create file shares that provide simultaneous access to data files, with direct input/output (I/O), through all the nodes in a file server cluster.
	- SMB Encryption. This feature provides the end-to-end encryption of SMB data on untrusted networks, and it helps to protect data from eavesdropping.
	- Windows PowerShell commands for managing SMB. You can manage file shares on the file server, end to end, from the command line.
	- SMB Multichannel. This feature enables you to aggregate network bandwidth and network fault tolerance if multiple paths are available between the SMB 3.x client and server.
	- SMB Direct. This feature supports network adapters that have the Remote Direct Memory Access (RDMA) capability and can perform at full speed with low latency and by using little central processing unit (CPU) processing time.

- SMB 3.1.1, which Microsoft introduced in Windows Server 2016, offers several additional enhancements, including:
	- Preauthentication integrity. Preauthentication integrity provides improved protection from a man-in-the-middle attack that might tamper with the establishment and authentication of SMB connection messages.
	- SMB Encryption improvements. SMB Encryption, introduced with SMB 3.0, uses a fixed cryptographic algorithm, AES-128-CCM. However, AES-128-GCM, available with SMB 3.1.1 performs better with most modern processors.
	- The removal of the RequireSecureNegotiate setting. Because some third-party implementations of SMB don't perform this negotiation correctly, Microsoft provides a switch to disable Secure Negotiate. However, the default for SMB 3.1.1 servers and clients is to use preauthentication integrity, as described earlier.

- Most common use cases of SMB 3 performance enhacements:
	- SMB Direct and SMB Multichannel enable you to deploy cost-efficient, continuously available, and high-performance storage for server applications on file servers. Both SMB Multichannel and SMB Direct are enabled by default on Windows Server. You can use multiple network connections simultaneously with SMB Multichannel, which enhances overall file-sharing performance. SMB Direct ensures that multiple network adapters can coordinate the transfer of large amounts of data at line speed while using fewer CPU cycles.
	- SMB Direct and SMB Multichannel-based file shares provide an alternative to storing files on Internet Small Computer System Interface (iSCSI) or Fibre Channel storage area network (SAN) devices. When creating a VM in Hyper-V on Windows Server, you can specify a network share when choosing the VM location and the virtual hard disk location. You can also attach disks stored on SMB 3.x file shares. By using this approach, you can achieve high availability not by clustering Microsoft Hyper-V nodes, but by using clustered file servers that host VM files on their file shares. This is referred to as a Scale-Out File Server. With this capability, Hyper-V can store all VM files, including configuration files, .vhd files, and checkpoints on highly available SMB file shares.
	- Cluster Dialect Fencing, available starting with SMB 3.1.1, provides support for cluster upgrades between consecutive operating system versions for the Scale-Out file Servers.

### CONFIGURE SMB PROTOCOL

- Windows server will negotiate and use the highest SMB version that a client (another server, a Windows 11 device, legacy client, Network-Attached Storage device) supports, which can result in protocol downgrade. Use of SMB1 should be blocked for security reasons as it was known for its vulnerability
- Enable SMB Encryption from Windows Admin Center

### DEFINE VOLUME SHADOW COPY SERVICE (VSS)

- Backing of critical business data can be a challenging task, primarily because of its size and high volume of changes. Effectively, some of data files might be open or they might be in an inconsistent state. To remediate this challenge, you can use VSS.

- Windows Server Backup uses VSS to perform backups. VSS coordinates the actions that are necessary to create a consistent shadow copy, also known as a snapshot or a point-in-time copy, of the data that's to be backed up. VSS solutions have the following basic components:
	- VSS service. This is part of the Windows operating system, which ensures that the other components can communicate with each other properly and work together.
	- VSS requester. This software requests the actual creation of shadow copies or other high-level operations like importing or deleting them. Typically, this is a backup application, such as Windows Server Backup.
	- VSS writer. This component guarantees that you have a consistent dataset to back up. This is typically part of a line-of-business application such as Microsoft SQL Server or Microsoft Exchange Server. The Windows operating system includes VSS writers for various Windows components such as the registry.
	- VSS provider. This component creates and maintains the shadow copies. This can occur in the software or in the hardware. The Windows operating system includes a VSS provider that uses copy-on-write. 

- to take advantage of VSS in combination with SMB file shares, both the SMB client and the SMB server must support SMB 3.0 at a minimum. VSS supports volume sizes of upto 64TB

## IMPLEMENT STORAGE SPACES AND STORAGE SPACES DIRECT

## IMPLEMENT WINDOWS SERVER DATA DEDUPLICATION

## IMPLEMENT WINDOWS SERVER ISCSI

## IMPLEMENT WINDOWS SERVER STORAGE REPLICA

## INTRODUCTION TO CLUSTER SHARED VOLUMES

