The AWS Cost and Usage Report (CUR) is a comprehensive reporting tool provided by Amazon Web Services (AWS) that delivers detailed, granular data about your AWS usage and costs. It generates customizable reports in `CSV` format, capturing billing, usage, and pricing details across all AWS services, accounts, and Regions.

Provides the **most detailed billing and usage data**, down to **hourly and per-resource granularity**. It’s the gold standard for tracking and controlling costs at a very fine level

Multi-Account Support: Aggregates data across AWS Organizations, ideal for tracking costs in multi-account setups

Granular Data: Tracks costs and usage at the resource level (e.g., specific EC2 instance, S3 bucket, KMS key) with hourly, daily, or monthly granularity.


| Line Item ID | Payer Account ID | Usage Account ID | Product Code | Resource ID                                                  | Usage Type                     | Start Date           | End Date             | Usage Amount | Pricing Unit | Unblended Cost | Net Unblended Cost | Project Tag | Availability Zone |
| ------------ | ---------------- | ---------------- | ------------ | ------------------------------------------------------------ | ------------------------------ | -------------------- | -------------------- | ------------ | ------------ | -------------- | ------------------ | ----------- | ----------------- |
| abc123       | 123456789012     | 987654321098     | AmazonRDS    | arn:aws:rds:us-east-1:987654321098:cluster:my-aurora-cluster | RDS:RunningHours:db.t4g.medium | 2025-10-01T00:00:00Z | 2025-10-01T01:00:00Z | 1            | Hrs          | 0.12           | 0.10               | Retail      | us-east-1a        |
| def456       | 123456789012     | 987654321098     | AmazonRDS    | arn:aws:rds:us-east-1:987654321098:cluster:my-aurora-cluster | RDS:Storage:gp2                | 2025-10-01T00:00:00Z | 2025-10-02T00:00:00Z | 10           | GB-Mo        | 1.00           | 0.90               | Retail      | us-east-1a        |
| ghi789       | 123456789012     | 987654321098     | AmazonRDS    | arn:aws:rds:us-east-1:987654321098:cluster:my-aurora-cluster | DataTransfer-Out-Bytes         | 2025-10-01T00:00:00Z | 2025-10-01T01:00:00Z | 1073741824   | Bytes        | 0.02           | 0.02               | Retail      | us-east-1         |
| jkl012       | 123456789012     | 987654321098     | AmazonRDS    | arn:aws:rds:eu-west-1:987654321098:cluster:my-global-cluster | RDS:RunningHours:db.t4g.medium | 2025-10-01T00:00:00Z | 2025-10-01T01:00:00Z | 1            | Hrs          | 0.14           | 0.12               | Retail      | eu-west-1a        |