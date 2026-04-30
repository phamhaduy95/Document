AWS Budgets cho phép user đặt ==một alert== cho một hạn mức sử dụng hoặc giới hạn chi phí của một AWS resource. Khi hạn mức này bị vượt qua, AWS Budget sẽ thông báo đến user qua Email hoặc **SNS** service.

Ngoài ra, user còn có thể tạo Action (Budget Action) cho phép user apply một IAM policy hay service control policy (SCP) để ngăn ngừa chi phi vượt quá xa.
Ví dụ:
Apply a custom `Deny IAM` policy that restricts the ability for a user, group, or role to provision additional Amazon EC2 resources. Target specific Amazon EC2 instances in `US East (N.Virginia) us-east-1`.

Các usage type được AWS budget hỗ trợ bao gồm:
- **Amazon RDS (including Aurora)**:
	- `RDS:RunningHours`: Instance hours for RDS/Aurora clusters (e.g., `db.t4g.medium` hours).
	- `RDS:Storage`: Storage GB-month for Aurora or RDS volumes.
	- `RDS:IOPS`: Provisioned IOPS for RDS (not Aurora, which auto-scales IOPS).
- **Amazon EC2**:
	- `BoxUsage`: Instance hours (e.g., `BoxUsage:t3.micro`).
	- `DataTransferOutBytes` : Outbound data transfer.
- **Amazon S3**:
	- `TimedStorage-ByteHrs`: Storage GB-month.
	- `Requests-Tier1`, `Requests-Tier2`: GET/PUT requests

AWS Budget theo dõi cost và usage cho toàn bộ service chứ không theo resource hay instance riêng lẻ. Tuy nhiên user có thể apply filter rule nhằm dẽ dàng tìm kiếm usage type cần theo dõi.

AWS Budget được integrated trực tiếp trên AWS Organization cho phép user đặt service usage threshold cho toàn bộ organization hay từng account riêng lẽ.

**Pricing**: Free to use.