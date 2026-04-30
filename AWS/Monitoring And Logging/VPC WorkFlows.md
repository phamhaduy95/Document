Amazon VPC Flow Logs is a feature that **captures information about the IP traffic that goes to and from network interfaces** in a Virtual Private Cloud (VPC)
It does not capture the content of the network traffic, only the metadata

A typical flow record includes:

- **Source and destination IP addresses and ports**: Identifies the origin and endpoint of the traffic.
- **Protocol**: Specifies the communication protocol, such as TCP, UDP, or ICMP.
- **Packets and bytes**: Records the amount of data transferred in a flow.
- **Time interval**: Marks the start and end times for the captured flow.
- **`Action`**: Indicates whether the traffic was `ACCEPT`ed or `REJECT`ed by security groups or network ACLs.
- **VPC, subnet, or network interface (ENI) ID**: Specifies the monitored resource

you can specific the scope for monitoring: VPC, subnet or even ENI

you can choose where to publish the logs.
- **Amazon CloudWatch Logs**: Useful for real-time monitoring and analytics with CloudWatch tools.
- **Amazon S3**: A good option for long-term storage and cost-effective archiving
- **Amazon Data Firehose**: Allows for streaming the logs to other analytics tools or destinations.
#### Application

VPC Flow Logs are valuable for a variety of tasks: 

- **Troubleshooting network connectivity**: You can diagnose overly restrictive security group rules or network ACLs by examining rejected traffic.
- **Monitoring traffic to instances**: Understand which instances are receiving the most traffic, from where, and on which ports.