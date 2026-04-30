Introduction
operational management service that provides a centralized platform for managing and automating operational tasks across your AWS infrastructure, as well as in hybrid and multicloud environments.

 AWS Systems Manager gives you visibility and control of your infrastructure on AWS. Systems Manager provides a unified user interface so you can view operational data from multiple AWS services and allows you to automate operational tasks across your AWS resources
**Unified console**
The unified console provides a centralized experience to view and manage your nodes. This console leverages several Systems Manager tools and more to provide you with the following:
- Centralized views of your nodes
- Detailed node insights
- Automated diagnosis and remediation of common node issues

what task can I perform with AWS system manager?

Key features of AWS Systems Manager
Systems Manager offers a collection of capabilities that are organized into categories based on operational needs: 
Operations management
- **Explorer:** A customizable dashboard quản lý các operation data như 
	1. Visualize your infrastructure 
	
- **OpsCenter:** A central hub for operations engineers and IT professionals to view, investigate, and resolve operational issues. It automatically creates OpsItems based on events from AWS services, so you can track and manage resolution efforts.
- **Incident Manager:** An incident management framework that helps your team triage and resolve critical incidents faster. It combines user engagement, escalation plans, runbooks, and chat channels to help manage major incidents. 
- **Patch policies** – Configure patching operations across multiple AWS accounts and Regions using a single policy through integration with AWS Organizations.
- **Custom patch baselines** – Define rules for auto-approving patches within days of their release, along with approved and rejected patch lists.
- **Multiple patching methods** – Choose from patch policies, maintenance windows, or on-demand "Patch now" operations to meet your specific needs.

Change management
- **Change Manager:** A framework for managing operational changes to application configurations and infrastructure.
- **Change Calendar:** Defines time windows for automated changes to prevent disruptions. 
	Node management
- **Session Manager:** Provides a secure browser-based shell and CLI for managing instances, eliminating the need for open ports or SSH keys.
- **Run Command:** Securely executes commands on managed nodes at scale without direct login.
- **State Manager:** Automates server configuration consistency and compliance.
- **Patch Manager:** Automates patching for security and other updates. It allows defining patch baselines and scheduling maintenance windows.
You can use Patch Manager to apply patches for both operating systems and applications.
- **Inventory:** Automatically collects software inventory from managed nodes.
- **Fleet Manager:** A graphical interface for managing servers and performing administrative tasks. 
Application management
- **Application Manager:** A centralized console for managing and monitoring applications and their resources.
- **AppConfig:** Safely and quickly deploys application configurations with validation checks. 
Shared resources
- **Parameter Store:** A centralized, hierarchical store for configuration data and secrets, which can be encrypted.
- **Automation:** Defines and executes runbooks for automating maintenance and deployment tasks.
- **Distributor:** Packages and distributes software to managed nodes.		
AWS system manager cho phép user register một action lên một hay nhiều AWS (individual or in bath) resource manually hoặc automatically. 

AWS hổ trợ các tác vụ sau:
- **Run Command:** Lets you remotely and securely manage the configuration of your instances.
- **Session Manager:** Provides secure access to your EC2 instances, replacing the need for SSH keys or bastion hosts.
- **Patch Manager:** Automates the patching of operating systems.
- **Inventory:** Gathers and reports inventory data on your managed instances.
- **State Manager:** Automates the process of keeping your instances in a defined state.
- **Maintenance Windows:** Allows you to schedule when potentially disruptive tasks are run. 
	- Automation

				
With Fleet Manager, you can view the health and performance status of your entire server fleet from one console. You can also gather data from individual nodes to perform common troubleshooting and management tasks from the console. This includes connecting to Windows instances using the Remote Desktop Protocol (RDP), viewing folder and file contents, Windows registry management, operating system user management, and more.

User có thể reuse lại các action này cho những lần sau.
có 3 loại actions chính:
- automation
- command
- policy

node manager
EC2 được install bởi agent mặc đinh
cần tạo role có để cấp quyền EC2 

debugging


**automation** là các tác vụ administration cho các AWS resource như start, stop nhiều EC2 instance cùng 1 lúc, xóa các temporary files trong S3. 
Automation, a tool in AWS Systems Manager, simplifies common maintenance, deployment, and remediation tasks for AWS services like Amazon Elastic Compute Cloud (Amazon EC2), Amazon Relational Database Service (Amazon RDS), Amazon Redshift, Amazon Simple Storage Service (Amazon S3), and many more.
Automation helps you to build automated solutions to deploy, configure, and manage AWS resources at scale. With Automation, you have granular control over the concurrency of your automations. This means you can specify how many resources to target concurrently, and how many errors can occur before an automation is stopped.
rate control


AWS develops and maintains several pre-defined runbooks.
Each AWS account can run 100 automations simultaneously.
Patch Manager helps you automate the patching of your Linux and Windows instances. It  
will work for supporting versions of the following operating systems

Depending on your use case, you can use these pre-defined runbooks that perform a variety of tasks, or create your own custom runbooks that might better suit your needs.
run command: 

Các task có thể thực hiện thông qua
run commands: chạy shell 
`AmazonSSMFullAccess` policy to grant access to 


#### Run command

#### Inventory




Inventory


AWS Systems Manager offers similar capabilities for automating configuration management, operations, and resource deployment, but it is more flexible and deeply integrated with modern AWS services like Amazon EC2 Auto Scaling