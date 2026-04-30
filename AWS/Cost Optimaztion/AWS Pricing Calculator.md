The AWS Pricing Calculator is a free, web-based tool provided by Amazon Web Services (AWS) that enables users to **estimate and forecast the costs of running AWS services**

- **Custom Estimations**: Select services, configure parameters (e.g., instance types, storage volumes, data transfer volumes), and apply usage patterns (e.g., on-demand, reserved instances) to generate monthly/annual estimates.
- **Scenario Comparison**: Create multiple scenarios (e.g., "Baseline EC2 Setup" vs. "Optimized with Savings Plans") to compare costs and identify savings opportunities.
- **Global Coverage**: Estimates costs across AWS Regions, with support for taxes, support plans, and currency conversion (e.g., USD to EUR)
- **Optimization Insights**: Suggests cost-saving options like Reserved Instances (RIs), Savings Plans, or Spot Instances during configuration.

#### Applications 

The Pricing Calculator is used by developers, architects, and finance teams to plan AWS deployments without surprises. Common applications include:
- **Pre-Deployment Planning**: Estimate costs for a new web app (e.g., your insurance quote application with EC2, RDS, and SQS) before launch.
- **Cost Optimization**: Model "what-if" scenarios, such as switching from On-Demand EC2 to RIs for oversized instances (as in your EC2 auditing query) or adding KMS for envelope encryption.
- **Disaster Recovery Budgeting**: Calculate multi-Region costs for DataSync transfers or Global Accelerator failover setups.
- **Compliance and Auditing**: Factor in services like Security Hub or Secrets Manager to ensure budgeted security controls.
- **Scaling Projections**: Forecast costs for growing workloads, e.g., increasing S3 storage for quote data or KMS API calls for Secrets Manager rotations.
- **Enterprise Reporting**: Generate detailed reports for stakeholders, including breakdowns by service, Region, or purchase option.