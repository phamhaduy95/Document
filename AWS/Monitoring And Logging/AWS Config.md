#### Introduction

AWS Config is a service that manage:

- **Configuration history:** It records every configuration change made to a resource, providing a historical record of its state over time. You can use this to see what a resource's configuration looked like at any specific point.

- **Resource inventory:** AWS Config maintains a **detailed inventory of your AWS resources** and their configurations. This includes resource types (e.g., EC2 instances, S3 buckets), IDs, tags, and **relationships** between resources.

- **Compliance evaluation:** You can define rules to assess resource configurations for compliance with your internal policies, security best practices, or regulatory standards. If a resource becomes non-compliant, AWS Config can automatically flag it and trigger SNS notifications.

Example configuration file of EC2 instances:
``` JSON
{
  "resourceType": "AWS::EC2::Instance",
  "resourceId": "i-0a1b2c3d4e5f6g7h",
  "relationships": [
    {
      "resourceType": "AWS::EC2::SecurityGroup",
      "resourceId": "sg-0123456789",
      "relationshipName": "Is associated with SecurityGroup"
    },
    {
      "resourceType": "AWS::EC2::EBSVolume",
      "resourceId": "vol-1234567890",
      "relationshipName": "Has an EBS volume"
    },
    {
      "resourceType": "AWS::EC2::VPC",
      "resourceId": "vpc-fedcba01",
      "relationshipName": "Is associated with VPC"
    },
    {
      "resourceType": "AWS::EC2::ElasticIp",
      "resourceId": "eip-555aaaabbbb",
      "relationshipName": "Has an Elastic IP"
    }
  ]
}
```


**AWS Config** will store configuration data and snapshots in S3.



#### Application

**Auditing and compliance:** Provide audit trails by proving that resource configurations meet specific security requirements.
**Change management:** Use AWS Config to track all changes to your resources. When integrated with AWS CloudTrail, you can determine what changed, when it changed, and who made the change.
**Cost optimization:** Find and identify underutilized resources, like unattached Amazon EBS volumes or oversized Amazon EC2 instances, to help manage costs.

**For automated enforcement (Config):**
- Once you understand your optimization needs, use **AWS Config** to create rules that enforce your new, more efficient standards.
- For example, you could create a Config rule that checks for unencrypted EBS volumes or EC2 instances with public IP addresses, and integrate it with automated remediation through AWS Systems Manager. This helps prevent cost- and security-related issues in the future


 AWS service captures the initial state of your AWS resources (EC2 instances and related items to start, with others planned) and the relationships between them, and then tracks creations, deletions, and property changes for analysis, visualization, and archiving.

With AWS Config, you get full visibility in to the state of your AWS resources. You can watch them change over time, and you can view the full history of configuration changes for a resource.