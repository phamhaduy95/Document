The AWS Storage Gateway service enables hybrid storage between on-premises environments and the AWS Cloud. 
- It allows you to seamlessly ***store and retrieve data from AWS services*** like Amazon S3, Amazon EBS, and Amazon S3 Glacier using standard storage protocols, without modifying your existing application
- It provides ***low-latency performance by caching frequently*** accessed data on premises, while storing data securely and durably in Amazon cloud storage services

AWS Storage Gateway supports three storage interfaces: file, volume, and tape

1. **File Gateway**: This gateway connects to Amazon S3 or Amazon FSx for Windows File Server and uses NFS and SMB protocols. Files are stored as objects in S3 with a local cache for faster access. An example is using an S3 File Gateway to allow existing file-based applications to utilize cloud storage.
 2. **Volume Gateway**: This type uses the iSCSI protocol to present block storage volumes and connects to Amazon S3, with backups saved as Amazon EBS snapshots. It has two modes: cached volumes, which store primary data in S3 with a local cache; and stored volumes, which store the full dataset locally and back up to S3
3. **Tape Gateway**:
	- Used for backup with popular backup software
	- It's used for moving backups and archives to the cloud

**Applications**
- **Hybrid cloud storage**: **Extend on-premises storage** using cloud capacity.
- **Backup and archive**: Replace physical tapes with Tape Gateway, archiving to Amazon S3 Glacier.
- **Disaster recovery**: Create cloud backups with Volume Gateway's stored volumes for quick recovery.
- **Low-latency access**: Access AWS data with low latency using File Gateway or Volume Gateway's cached mode.