#### Accessing EC2 Instances via Bastion Host  
A bastion host is a secure server that acts as a gateway for accessing instances in private subnets. To connect using the AWS CLI: Launch a  Bastion Host: Deploy an EC2 instance in a public subnet with SSH access enabled. Ensure the bastion host has access to private resources (e.g., EC2  
instances) in the VPC.  
SSH to the Bastion Host: SSH into the bastion host from your local  
machine using its public IP address: ssh -i /path/to/key.pem ec2-user@bastion-public-ip  
SSH from the Bastion Host to the Private Instance: Once on the bastion  
host, use SSH to access the private instance in the VPC: ssh -i /path/to/key.pem  
ec2-user@private-instance-ip Run AWS CLI Commands: You can now run AWS  
CLI commands from the private instance to interact with other private  
resources like RDS databases or S3 VPC endpoints: aws s3 ls

Với window instance, ta vào AWS console extract password từ private key từ cặp key-pair. Đăng nhập bằng hình thức remote. 

#### Using AWS Systems Manager (SSM) Session Manager  
SSM Session Manager allows you to manage EC2 instances in a private  
subnet without opening inbound ports or needing a bastion host.  
Attach IAM Role with SSM Permissions: Attach an IAM role to your  
EC2 instance with the AmazonSSMManagedInstanceCore policy.  
Enable SSM Agent on the Instance: Ensure the SSM Agent is installed and running on the instance (pre-installed on Amazon Linux and Windows AMIs).  
Start a Session Using the AWS CLI: Start a session to the instance using  
its Instance ID: aws ssm start-session --target i-0123456789abcdef0  
Run Commands Within the Session: You can now run AWS CLI  
commands or shell commands directly on the instance.


Use IAM Roles for Authentication: Attach IAM roles to EC2 instances or  
Lambda functions instead of using hard-coded credentials.