#### Introduction

**Amazon Inspector** is a dedicated **vulnerability management** service which automatically scans AWS resources (like EC2 instances, ECR images, Lambda functions) for **software vulnerabilities** **** or unintended network exposure.

It can utilize the **AWS Systems Manager** (SSM) **agent**, which is already present on many AWS AMIs, for agent-based scanning. This method helps to identify software vulnerabilities within the operating system and applications.

When Amazon Inspector discovers a vulnerability, it creates a finding with detailed information, severity ratings, and remediation guidance.

It publishes these findings as events to **Amazon EventBridge**, which can be used to trigger automated actions, such as an **AWS Lambda function**.