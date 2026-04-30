EventBridge is a **serverless event bus** service that makes it easy to connect applications using **events** (real-time messages that describe changes in state).  
It’s an evolution of **Amazon CloudWatch Events** — and it’s used to build **event-driven architectures** in AWS.

| **Concept**      | **Description**                                                                                         |
| ---------------- | ------------------------------------------------------------------------------------------------------- |
| **Event**        | A JSON message that describes something that happened — e.g., “OrderCreated”, “EC2 instance started”.   |
| **Event Bus**    | A pipeline that receives, filters, and routes events to targets.                                        |
| **Rule**         | Defines a **pattern** to match certain events and routes them to one or more **targets**.               |
| **Target**       | The AWS service or resource that processes the event (e.g., Lambda, Step Functions, SNS, SQS, Kinesis). |
| **Event Source** | Where events come from — AWS services, SaaS integrations, or your own custom applications.              |
