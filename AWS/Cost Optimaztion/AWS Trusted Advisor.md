**AWS Trusted Adviso**r is a service that provides real-time recommendations to optimize your AWS resources, improve security, and reduce costs.
Trusted Advisor provides recommendations for:

- **performance Improving efficiency** and speed for Auto Scaling Group
- **security**: Protecting resources
- **fault tolerance**
- **service limits** 
- **Cost optimization check**s: identifies **unused or underutilized resources,** such as idle EC2 instances, unattached EBS volumes, or unused Elastic IPs, helping you eliminate waste and lower costs

Trusted Advisor operates at the **individual account level** for resource-specific checks
#####  Cost Optimization (Focus: Reducing unnecessary spend)

- **Idle DB Instances**: Identifies RDS instances with low utilization (e.g., <10% CPU over 7 days) that can be stopped or deleted to save costs.
- **Idle DB Connections**: Detects RDS instances with excessive idle connections, recommending optimization to lower resource waste.
- **Amazon EC2 Reserved Instance Optimization**: Analyzes EC2 usage to recommend RI purchases for savings up to 72% vs. On-Demand.
- **Amazon EC2 Instance Optimization**: Suggests rightsizing over-provisioned EC2 instances (e.g., switching to Graviton processors) or stopping unused ones.
- **Amazon EBS Over-Provisioned Volumes**: Flags EBS volumes with low IOPS/throughput usage, recommending smaller sizes or GP3 volumes for cost savings.
- **Amazon S3 Bucket with Public ACLs**: Identifies buckets with public access that could lead to unexpected data transfer fees.
- **Low-Utilization Amazon EC2 Instances**: Recommends terminating or resizing instances with <10% CPU and low network I/O over 14 days.
- **Amazon Redshift Reserved Node Optimization**: Suggests RIs for underutilized Redshift clusters.
##### Performance 

- **Amazon EC2 Instance in Single Availability Zone**: Warns if critical EC2 instances lack AZ diversity, impacting load balancing.
- **ELB Optimization**: Checks Elastic Load Balancers for even instance distribution across AZs to avoid hotspots.
- **Amazon EBS Volume High I/O**: Identifies volumes with latency issues, suggesting optimization or larger types.
- **Auto Scaling Group Configuration**: Ensures ASGs have proper min/max sizes and health checks for responsive scaling.
##### Security 

- **IAM Use**: Flags unused IAM users, roles, or access keys (>90 days inactive) to reduce breach risks.
- **MFA on Root Account**: Ensures multi-factor authentication is enabled for the root user.
- **Publicly Accessible RDS Instances**: Detects RDS DBs exposed to the internet without security groups.
- **S3 Bucket Public Read ACL**: Warns about buckets allowing public reads, risking data exposure.
- **Exposed Access Keys**: Scans for leaked AWS keys in public repos or suspicious EC2 activity.
- **EBS Public Snapshots**: Alerts on publicly shared EBS snapshots that could leak data.
##### Fault Tolerance (Resilience) 

- **ELB Across Multiple AZs**: Recommends spreading load balancers across AZs for failover.
- **RDS Multi-AZ Deployment**: Suggests enabling Multi-AZ for production RDS to handle outages.
- **EC2 Security Groups**: Ensures groups allow inbound traffic only from trusted sources.
- **Auto Scaling Implementation**: Checks if ASGs are configured to replace failed instances automatically.
##### Service Limits 

- **EC2 Reserved Instance Utilization**: Monitors if you're approaching RI lease limits.
- **RDS Total Storage Quota**: Alerts if storage usage exceeds 80% of quotas.
- **VPC Limits**: Tracks if you're nearing VPC, subnet, or ENI quotas.
