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

### DEFINE THE STORAGE SPACES ARCHITECTURE AND ITS COMPONENTS

- To make more efficient use of storage many organisations have implemented storage area networks (SANs). However most SANs require advanced configurations and expensive hardware. Storage Spaces is a solution that provides a viable alternative to SAN

- A storage space is a storage-virtualization capability built into Windows Server and Windows 10 and later. The Storage Spaces feature consists of two components:
	- Storage pools. A storage pool is a collection of physical disks aggregated into a logical disk that you can manage as a single entity. The pool can contain physical disks of any type and size. A single physical disk can belong to only one storage pool.
	- Storage Spaces. Storage Spaces are virtual disks created from free space in a storage pool. Storage Spaces provide such functionality as resiliency levels, including mirroring and parity, storage tiers, write-back caching, fixed and thin provisioning, and management controls. Virtual disks are equivalent to logical unit numbers (LUNs) on a SAN.

- To create a highly available virtual disk, you need at least one physical disk that satisfies the following requirements:
	- One physical disk is required to create a storage pool.
	- A minimum of two physical disks are required to create a resilient mirror virtual disk.
	- A minimum of three physical disks are required to create a virtual disk with resiliency through parity.
	- Three-way mirroring requires at least five physical disks.
	- Disks must be blank and unformatted. No volume can exist on the disks.
	- You can attach disks by using various bus interfaces, including, small computer system interface (SCSI), Serial Attached SCSI (SAS), Serial ATA (SATA), NVM Express (NVMe).

- Virtual disks resiliency resembles Redudant Array of Independent Disks (RAID) technologies, but Storage Spaces store the data differently than RAID. If you want to use failover clustering with storage pools you can't use SATA, USB or SCSI disks

- Storage tiers allow you to optimize the use of different disk types in a storage space. For example, you could use very fast but small-capacity solid-state drives (SSDs) with slower, but large-capacity hard disks. When you use this combination of disks, Storage Spaces automatically moves data that is accessed frequently to the faster disks, and then moves data that is accessed less often to the slower disks. By default, the Storage Spaces feature moves data once a day at 01:00 AM. You can also configure where files are stored. The advantage is that if you have files that are accessed frequently, you can pin them to the faster disk. The goal of tiering is to balance capacity against performance. Windows Server recognizes only two levels of disk tiers: SSD, and non-SSD. Windows Server 2019 added support for persistent memory (PMem). You can use PMem as a cache to accelerate the active working set, or as capacity to guarantee consistent low latency on the order of microseconds

- The purpose of write-back caching is to optimize writing data to the disks in a storage space. Write-back caching works with Tiered Storage Spaces. If the server that is running the storage space detects a peak in disk-writing activity, it automatically starts writing data to the faster disks. By default, write-back caching is enabled. Write-back cache has the size limit of 1GB

- Fixed provisioning allocates the storage capacity up front when you create the space. Thin provisioning enables storage to be allocated readily on a just-enough and just-in-time (JIT) basis. In this case, storage capacity in the pool is organized into provisioning slabs that aren't allocated until datasets require the storage. Instead of the traditional fixed storage allocation method in which large portions of storage capacity are allocated but might remain unused, thin provisioning optimizes any available storage by reclaiming storage that is no longer needed using a process known as trim. You can create both thin and fixed provisioning virtual disks within the same storage pool. Having both in the same storage pool is convenient, especially when they're related to the same workload. For example, you can choose to use a thin provisioning space for a shared folder containing user files, and a fixed provisioning space for a database that requires high disk I/O.

- You can manage Storage Spaces interactively by using File and Storage Services role in Server Manager, via command line with Windows PowerShell, or programmatically, via Windows Storage Management application programming interface (API) in Windows Management Instrumentation (WMI). After you have provisioned the virtual disks, you can make them available to the Windows operating system by creating disk drives and either mounting them onto a local file system directory or assigning to them a drive letter. You can format a storage space virtual disk with FAT32, NT File System (NTFS), or Resilient File System (ReFS)

### LIST THE FUNCTIONALITIES, BENEFITS, AND USE CASES OF STORAGE SPACES

- When considering whether to use Storage Spaces in each situation, you should consider their benefits. Storage Spaces allow you to:
	- Implement and easily manage scalable, reliable, and inexpensive storage.
	- Aggregate individual drives into storage pools, which are managed as a single entity.
	- Use inexpensive storage with or without external storage.
	- Use different types of storage in the same pool (for example, SATA, SAS, USB, and SCSI).
	- Grow storage pools as required.
	- Provision storage when required from previously created storage pools.
	- Designate specific drives as hot spares.
	- Automatically repair pools containing hot spares.
	- Delegate administration by pool.
	- Use the existing tools for backup and restore, and use Volume Shadow Copy Service (VSS) for snapshots.
	- Manage either locally or remotely, by using Microsoft Management Console (MMC) or Windows PowerShell.
	- Utilize Storage Spaces with Failover Clusters.

- Limitations of Storage Spaces:
	- Storage Spaces volumes aren't supported as boot or system volumes.
	- You should add only unformatted, non-partitioned, disks to a storage pool.
	- All drives in a pool must use the same sector size.
	- Storage layers that abstract the physical disks aren't compatible with Storage Spaces, including:
	- Pass-through disks in a virtual machine (VM).
	- Storage subsystems deployed in a separate RAID layer.
	- Fibre Channel and Internet Small Computer System Interface (iSCSI) aren't supported.
	- Failover Clusters are limited to SAS as the storage technology.

- Workload types and resiliency types
| **Resiliency type** | Number of data copies maintained        | **Workload recommendations**                                                       |
|---------------------|-----------------------------------------|------------------------------------------------------------------------------------|
| Mirror              | 2 (two-way mirror) 3 (three-way mirror) | Recommended for all workloads                                                      |
| Parity              | 2 (single parity) 3 (dual parity)       | Sequential workloads with large units of read/write, such as archival              |
| Simple              | 1                                       | Workloads that don't need resiliency, or provide an alternate resiliency mechanism |

- Thin provisioning optimizes utilization of available storage. This can be a factor contributing to datacenter consolidation, resulting in lower operating costs. The downside of using thin provisioning, when compared with fixed provisioning, is slightly decreased storage performance.

### IMPLEMENT STORAGE SPACES

- As part of the implementation of Storage Spaces, you need to provision an appropriate amount and type of storage. You also need to choose the optimal logical sector sizes, drive allocation algorithm, and provisioning scheme.

- The assignment of the logical sector size takes place when you create a storage pool. If you use only 512 and/or 512e drives, then the pool defaults to 512e. A 512 drive uses 512-byte sectors. A 512e drive is a hard disk with 4,096-byte sectors that emulates 512-byte sectors. If the list contains at least one 4-kilobyte (KB) drive, then the pool sector size is 4 KB by default. Optionally, you can explicitly specify the sector size of a pool and enforce it for all of its spaces. Your choices include 512 or 512e for a 512e storage pool, and 512, 512e, or 4 KB for a 4-KB pool.

- You can configure how a pool allocates drives. Options include:
	- Automatic. This is the default allocation when you add any drive to a pool. Storage Spaces can automatically select available capacity on data-store drives for both storage-space creation and JIT allocation.
	- Manual. You can specify Manual as the usage type for drives that you add to a pool. A storage space won't use a manual drive automatically unless you select it when you create that storage space. This property makes it possible for administrators to specify that only certain Storage Spaces can use particular types of drives.
	- Hot spare. Drives that you add as hot spares to a pool aren't available when creating a storage space. Instead, their purpose is to automatically replace failed drives.

- You can provision a virtual disk by using one of two provisioning schemes:
	- Fixed provisioning. With fixed provisioning, Storage Spaces allocates all storage capacity you designate at the time that you create the storage space.
	- Thin provisioning. With thin provisioning, Storage Spaces allocates storage on as-needed basis, as dataset grows, up to the limit you designate.

### LIST THE FUNCTIONALITIES, COMPONENTS, BENEFITS, AND USE CASES OF STORAGE SPACES DIRECT

- Storage Spaces Direct is the evolution of Storage Spaces, first introduced in Windows Server 2012. It leverages Storage Spaces, Failover Clustering, Cluster Shared Volumes (CSVs), Software Storage Bus, and SMB 3.x to implement virtualized, highly-available shared storage by using local disks on each of the Storage Spaces Direct cluster nodes. It's suitable for hosting highly-available workloads, including virtual machines and SQL Server databases. Storage Spaces Direct supports both direct-attached storage (DAS) and JBODs. This eliminates the need for a shared storage fabric and enables you to use a mix of SATA disks to lower costs and NVMe devices to improve performance.
- CSV is a clustered file system that enables cluster nodes to simultaneously read from and write to the same set of NTFS or ReFS volumes. This provides a balanced load distribution and increases failover speed by eliminating the need for drive ownership changes or dismounting and remounting volumes. Software Storage Bus forms a software-defined storage fabric consisting of local drives on cluster nodes. It also dynamically binds the fastest drives to slower drives to provide server-side read/write caching that accelerates I/O operations and increases throughput.

- The architecture of Storage Spaces Direct consists of the following components:
	- Storage Spaces Direct workloads. Common workloads include VMs and SQL Server databases.
	- CSV. CSVs consolidate multiple volumes into a single namespace that's accessible through the file system on any cluster node.
	- ReFS or NTFS-formatted volumes. ReFS is the recommended option because it accelerates virtual hard disk (VHD/VHDX) operations, providing superior performance in comparison with NTFS. ReFS also offers resiliency benefits such as error detection and automatic correction.
	- Storage Spaces and the underlying virtual disks. With Storage Spaces, you create virtual disks by using available storage in the storage pool. Virtual disks provide resiliency against disk and server failure because data is distributed across disks on different servers.
	- In the context of Storage Spaces Direct, the term volume typically refers to the volume and the underlying virtual disk.
	- Software Storage Bus. Storage Spaces Direct uses Server Message Block (SMB) for intranode communication by using Software Storage Bus. Software Storage Bus exposes the storage on each node, making it part of the Storage Spaces layer.
	- Failover clustering. Failover clustering is the Windows Server feature that allows you to implement highly available workloads, including Storage Spaces Direct.
	- Windows Server instances. Storage Spaces Direct cluster can include between 2 and 16 servers.
	- SMB Networking. SMB networking includes support for SMB Direct and SMB Multichannel.
	- Network. Storage Spaces Direct requires network connectivity between cluster nodes. You should use multiple, RDMA-capable network adapters per node.
	- Storage pool. The storage pool uses local disks from all cluster nodes.
	- Local disks. Each server must have locally-attached storage, such as HDD, SSD, NVMe, or PMem disks.

- Windows Server offers a range of Storage Spaces Direct-related benefits, including:
	- Deduplication and compression for ReFS volumes. Deduplication supports volumes up to 64 terabytes (TB) and will deduplicate the first 4 TB of each file.
	- Native support for persistent memory modules in Storage Spaces Direct clusters. You can use persistent memory as cache to accelerate the active working set, or as capacity to guarantee consistent, low latency on the order of microseconds.
	- Nested resiliency for two-node hyper-converged infrastructure. With nested resiliency, a two-node Storage Spaces Direct cluster can provide continuously accessible storage for apps and VMs even if one server node stops working and a drive fails in the other server node.
	- USB flash drive as a witness. You can use a low-cost USB flash drive that's plugged into your router to function as a witness in two-node Storage Spaces Direct clusters.
	- Performance history for visibility into resource utilization and performance. This built-in functionality automatically collects over 50 essential counters spanning compute, memory, network, and storage and stores them for up to one year.
	- Scaling for up to 4 petabytes (PB) per cluster. Starting with Windows Server 2019, Storage Spaces Direct supports up to 4 PB (or 4,000 TB) of raw capacity per storage pool.
	- Mirror-accelerated parity. With mirror-accelerated parity you can create Storage Spaces Direct volumes that are part mirror and part parity, similar to mixing RAID-1 and RAID-5/6 to combine their benefits. In Windows Server 2019, mirror-accelerated parity performance more than doubled compared to Windows Server 2016.
	- Drive latency outlier detection. Storage Spaces Direct automatically detects anomalous changes in drive performance and labels them in Windows PowerShell and Windows Admin Center with the Abnormal Latency status.
	- Storage-class memory support for VMs. This enables NTFS-formatted direct access volumes to be created on non-volatile dual inline memory modules (DIMMs) and exposed to Microsoft Hyper-V VMs. This enables Hyper-V VMs to take advantage of the low-latency performance benefits of storage-class memory devices.
	- Windows Admin Center extensions. You can manage and monitor Storage Spaces Direct with purpose-built dashboards in Windows Admin Center.

When planning for Storage Spaces Direct, you need to determine whether you want to separate the virtualization and storage layers. This separation determines whether you'll implement hyper-converged or disaggregated architecture.
	- In the hyper-converged architecture, you configure a Hyper-V cluster with local storage on each Hyper-V server, and scale this solution by adding extra Hyper-V servers with extra storage. This is the optimal solution for small and medium-sized businesses.
	- If you need to be able to scale the virtualization layer and the storage layer independently of each other, you should choose the disaggregated architecture. This approach consists of two separate clusters, with one hosting Hyper-V VMs and the other a Scale-Out File Server (SOFS) where the VM disks reside. This solution lets you scale processing power for the virtualization layer separately from storage capacity for the storage layer. This is typically optimal for large-scale deployments.
	- Other use cases for Storage Spaces Direct include storage of Hyper-V Replica files and for backup or archival of VM files. You can also deploy Storage Spaces Direct to host Microsoft SQL Server system and user database files.

## IMPLEMENT WINDOWS SERVER DATA DEDUPLICATION

## IMPLEMENT WINDOWS SERVER ISCSI

## IMPLEMENT WINDOWS SERVER STORAGE REPLICA

## INTRODUCTION TO CLUSTER SHARED VOLUMES

