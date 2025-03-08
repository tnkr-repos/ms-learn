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
- Open `Windows Administrative Tools` -> `File Server Resource Manager` -> Right click and connect to another computer


### DEFINE SMB AND ITS SECURITY CONSIDERATIONS

### CONFIGURE SMB PROTOCOL

### DEFINE VOLUME SHADOW COPY SERVICE (VSS)

## IMPLEMENT STORAGE SPACES AND STORAGE SPACES DIRECT

## IMPLEMENT WINDOWS SERVER DATA DEDUPLICATION

## IMPLEMENT WINDOWS SERVER ISCSI

## IMPLEMENT WINDOWS SERVER STORAGE REPLICA

## INTRODUCTION TO CLUSTER SHARED VOLUMES

