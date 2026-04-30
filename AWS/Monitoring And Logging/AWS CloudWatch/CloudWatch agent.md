The **CloudWatch Agent** is a **software agent** that runs on **EC2 instances**, **on-prem servers**, or **containers**, and sends **system-level metrics and logs** to Amazon CloudWatch.

It gives you **deeper visibility** than the default metrics AWS provides.

By default, EC2 instances send **basic metrics** to CloudWatch (CPU, network, disk I/O).  
But sometimes you want more detailed info, such as:

- Memory usage 🧠
- Disk space utilization 💽
- Swap usage
- Custom app logs (e.g., `/var/log/myapp.log`)

The CloudWatch Agent can collect and publish those to CloudWatch **Metrics** or **Logs**.


| **Type**    | **Example**                                   | **Destination**    |
| ----------- | --------------------------------------------- | ------------------ |
| **Metrics** | Memory %, DiskSpaceUsed, SwapUsed, Processes  | CloudWatch Metrics |
| **Logs**    | Application log, system log, custom log files | CloudWatch Logs    |