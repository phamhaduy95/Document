AWS Control Tower is ==a management and governance service that provides a simple and automated way to set up and govern a secure, multi-account AWS environment==. It is built on top of other AWS services, including AWS Organizations, AWS Service Catalog, and IAM Identity Center

The functionality of AWS Organizations can be extended through “landing zones” generated  by Control Tower. Landing zones streamline the onboarding of new accounts, automatically  applying your organization’s governance policies and incorporating them into your cloud  infrastructure.

Landing zone
AWS Control Tower also automates the creation of a landing zone for onboarding  
AWS accounts using pre-built blueprints that follow suggested best practices for configuring a default identity, federated access, and account structure


Controls (Guardrails)

Controls are pre-packaged governance rules that help enforce security, operational, and compliance policies. Control Tower offers three types of controls: 

- **Preventive controls:** These use Service Control Policies (SCPs) to stop non-compliant actions from happening. For example, a preventive control can deny the creation of S3 buckets with public access.
- **Detective controls:** These use AWS Config rules to continuously monitor and detect non-compliant resources. For example, a detective control can notify you if an S3 bucket is found to have public access.
- **Proactive controls:** These prevent a non-compliant resource from being provisioned by running a check via AWS CloudFormation Hooks.

Account Factory

This is a self-service account provisioning tool that uses templates in AWS Service Catalog to standardize the creation of new AWS accounts. The Account Factory automates the process, ensuring all new accounts are created with pre-approved configurations and immediately governed by the controls you have enabled.


How Control Tower extends AWS Organizations

AWS Control Tower builds upon the foundational capabilities of AWS Organizations. While Organizations helps you manage multiple accounts and use SCPs, Control Tower provides a higher level of automation and a prescriptive approach.

- **Automation:** Control Tower automates the setup of a secure, compliant multi-account structure, including the core accounts for logging and auditing, and a federated identity solution with IAM Identity Center.
- **Best Practices:** It enforces a set of mandatory and optional controls based on AWS best practices. This helps prevent configuration drift and ensures your environment adheres to security standards from the start.
- **Unified View:** The Control Tower dashboard gives you a centralized view of compliance and security across all your governed accounts, which is a feature not natively provided by AWS Organizations alone.