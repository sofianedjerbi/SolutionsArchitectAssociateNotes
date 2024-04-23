# Storage Extras

## FSx

Both are available in SSD and HDD and can be used **on premises**.

### FSx for Windows

Supports SMB, DFS and NTFS.  
Can **be mounted on linux**. Can be multi-AZ.  

### FSx for Lustre

A **parallel distributed file system** for HPC.  
**Seamless integration with S3.**

### FSx for NetApp ONTAP

NetTap ONTAP on AWS. Works with almorst every OS.  
Compatible with NFS, SMB, iSCSI.  
Storage automatically shrinks or grows.  
**Point in time instantaneous cloning**.

### FSx for OpenZFS

Compatible with NFS.  
Use case: Move ZFS workloads on AWS.  
**Point in time instantaneous cloning**.

### Deployment Options

- **Persistent:** Long-term storage but within the **same AZ**.
- **Scratch:** Temporary storage but 6x faster.

## Storage Gateway

Type of storage gateway :

- **S3:** Link to S3 with SMB / NFS and HTTPS as a backend. Local cache recently used data. **(no glacier)**
- **FSx:** Link to FSx. Local cache for recently used data.
- **Volume:** Block **backup** storage using iSCSI protocol backend by S3. Local cache and **point in time backups**.
- **Tape:** **Backup** data to a virtual tape library backed by **S3 and Glacier**.

SMB compatibility implies that it is compatible with **Active Directory**.  
The file / volume gateway cam be setup **on premises** and linked to **storage gateway**.  
**Hardware appliance** is available.  

## Transfer Family

Managed service for **FTP (only within VPC)/FTPS/SFTP in and out of S3 or EFS**.  
Scalable, multi-AZ. Pay per **provisioned endpoint per hour + data transfers in GB**.  
Store and manage credentials within the service.

## DataSync

Synchronize data to and from places. Like **rsync**.  
On-Premises (DataSync Agent) <-> AWS  
AWS <-> AWS (S3, FSx, EFS)  
**Permissions and metadata are preserved**.

## Storage Comparison

- **S3:** Object Storage
- **S3 Glacier:** Object Archival
- **EBS volumes:** Network storage for one EC2 instance at a time
- **Instance Storage:** Physical storage for your EC2 instance (high IOPS)
- **EFS:** Network File System for Linux instances, POSIX filesystem
- **FSx for Windows:** Network File System for Windows servers
- **FSx for Lustre:** High Performance Computing Linux file system
- **FSx for NetApp ONTAP:** High OS Compatibility
- **FSx for OpenZFS:** Managed ZFS file system
- **Storage Gateway:** S3 & FSx File Gateway, Volume Gateway (cache & stored), Tape Gateway
- **Transfer Family:** FTP, FTPS, SFTP interface on top of Amazon S3 or Amazon EFS
- **DataSync:** Schedule data sync from on-premises to AWS, or AWS to AWS
- **Snowcone / Snowball / Snowmobile:** To move large amount of data to the cloud, physically
- **Database:** for specific workloads, usually with indexing and querying
