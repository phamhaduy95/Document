
- Recovery point objective (RPO): RPO is defined as the maximum acceptable amount of time since the last data recovery point or the acceptable loss of data. If your RPO is defined as 5 minutes, then the disaster recovery process needs to be designed to restore the data records to within 5 minutes of when the disaster occurred.  
- Recovery time objective (RTO): RTO is defined as the maximum acceptable delay between a service interruption and a return to normal service. If RTO is set to 1 hour, for example, the workload should be up and functional within that 1-hour timeframe.

![[Disaster Recovery Strategies.png]]

Customers need to define acceptable values for RPO and RTO, defining a recovery strategy that meets the recovery objectives of each workload. Strategies chosen include active-passive (backup and restore, pilot light, or warm standby), or multisite active-active (see Figure 7-22)

Backup and restore

Backup and Restore Backup and restore strategies back up data and workloads into a DR location. The DR location could be a separate AWS region.
Using a Multi-AZ strategy within a single AWS region may be sufficient for some workloads; it will depend on the specific use case and business requirements.

AWS service áp dụng strategy này
EBS snapshot
Amazon Machine Image

**Pilot Light**
A disaster recovery pilot light is a disaster recovery strategy in which a minimal  system is kept running at all times, ready to be expanded into a full-scale workload  
in the event of a disaster. Pilot light deployments could have synchronized database records and idle compute services. For example, a pilot light disaster recovery strategy has a defined online primary site where web and primary database servers are operating and are fully operational.

![[Pasted image 20251003180053.png]]


Warm standby

A warm standby solution speeds up the recovery time because all the components in  
the warm standby location are already in active operation—hence the term warm—  
but at a smaller scale of operation when compared to the primary site.

Because warm standby resources were already active, the recovery time  
is shorter than with a pilot light solution; however, a warm standby solution is more  
expensive than a pilot light option as additional standby resources are running 24/7

AWS Backup can back up  
Amazon EC2 instances, Amazon EBS volumes, Amazon S3 buckets, RDS deployments, Amazon EFS, Amazon FSx for Windows File Server, Amazon DynamoDB  
tables, and VMware workloads on premises, on Amazon Outposts, and hosted in  
VMware Cloud on AWS.
