
Amazon GuardDuty is a near-real-time **threat detection service** that continuously monitors and  protects your AWS account, EC2 instances, container applications, databases, and data stored in S3 buckets.

Tìm kiếm các security thread như xâm nhập bất hợp lệ, malware từ bên ngoài.

It uses ML to analyze data and logs from:

- CloudTrail events
- VPC flow logs: traffic 
- Amazon Elastic Kubernetes Service audit logs
- DNS query logs
- CloudWatch logs
- S3 data events
- RDS logs: brute-force attacks or unauthorized access

to detect threats such as:

- Compromised instances
- API abuse
- credential abuse
- reconnaissance by attackers
- **malware**
- suspicious traffic 
- **malicious activity** 
- **unauthorized behavior**

AWS GuardDuty provides alerts for any suspicious activity it detects, allowing organizations to take 
appropriate action to protect AWS resources.

Amazon GuardDuty can also be deployed with AWS Organizations (AWS recommended deployment)

When Amazon GuardDuty Malware Protection finds issues with EBS volumes, it creates replica snapshots of the affected EBS volumes
#### Integration

**Security Hub**: ingests GuardDuty findings to provide a centralized dashboard for security posture assessment against industry standards

**EventBridge**: routes GuardDuty findings as events to trigger automated workflows.

**CloudWatch:** GuardDuty exports metrics (e.g., finding counts) and logs to CloudWatch for monitoring and alerting

#### Application
**Detecting compromised instances**: GuardDuty can identify activity like cryptocurrency mining, communication with malicious IP addresses, or unusual spikes in network traffic originating from an EC2 instance.

**Identifying account compromises**: It can flag suspicious API calls from an unusual location, attempts to disable CloudTrail logging, or unexpected changes to IAM policies.

**Protecting S3 data**: GuardDuty can detect unusual access patterns to your S3 buckets, such as data being accessed from a known malicious IP address or large-scale data retrieval.

**Safeguarding container workloads**: It can monitor for suspicious activity in your Amazon EKS and ECS clusters.

**Malware Protection**: It can detect malware by scanning EBS volumes attached to EC2 instances and by scanning new objects uploaded to S3 buckets