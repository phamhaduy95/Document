investigate the security findings
- Detective uses machine learning and graph theory to create a "behavior graph," which is an interactive visualization of your resources, users, and their interactions over time.
- This graph helps a security analyst visualize and understand the scope of an incident, track down the root cause, and identify the impacted resources.

**Data Ingestion**:
- Collects logs from CloudTrail (management and data events), VPC Flow Logs, GuardDuty findings, EKS audit logs, and S3 access logs.
- Supports AWS Organizations for multi-account data aggregation.

**Graph Creation**:
- Builds a behavior graph modeling interactions (e.g., an EC2 instance’s API calls or network connections).
- Uses machine learning to identify anomalies and link related events.

**Investigation**:

- Start from a GuardDuty finding, Security Hub alert, or manually select a resource (e.g., an RDS instance).
- Visualize timelines, IP addresses, and affected resources to trace malicious activity.

**Root Cause Analysis:** This process helps the analyst quickly determine if the activity was malicious or a false positive, assess its impact, and discover the underlying cause without manually sifting through log files