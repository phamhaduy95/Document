#### Introduction
Amazon Kinesis Data Streams (KDS) is ==a fully managed, serverless streaming data service used to capture, process, and store large volumes of data records in real-time==. It provides a highly durable and scalable platform for building custom applications that process or analyze streaming data from hundreds of thousands of sources. 
#### Features

- **Massively Scalable**: Kinesis Data Streams can continuously ingest gigabytes of data per second. You can scale your stream's capacity to handle your workload by managing shards in either on-demand or provisioned mode.
- **Durable and Highly Available**: The service synchronously replicates data across three Availability Zones (AZs) within an AWS Region. It can store data for up to 365 days, providing high availability and protection against data loss.
- **Low Latency**: Ingested data is available for processing within milliseconds, enabling real-time analytics and fast responses to new information.
- **Enhanced Fan-Out**: This feature allows multiple consumers to read data from the same stream in parallel without competing for read throughput. Each consumer gets its own 2 MB/second allotment of read throughput per shard, ensuring low-latency delivery.
- **Security**: Kinesis Data Streams supports server-side encryption with AWS Key Management Service (KMS) to protect sensitive data at rest. It also integrates with AWS Identity and Access Management (IAM) for controlling access.
- **Fault-Tolerant Consumption**: The Kinesis Client Library (KCL) helps you build fault-tolerant applications by managing checkpoints and ensuring that each record is processed only once.

#### Mode
Kinesis Data Streams offers two distinct capacity modes to manage throughput, which directly impacts your pricing:

- **On-demand mode**: In this serverless mode, Kinesis Data Streams automatically manages and scales the throughput capacity for you. You don't need to specify the number of shards, and you pay for the volume of data you ingest and retrieve. This is ideal for applications with unpredictable traffic patterns.
- **Provisioned mode**: You manually specify the number of shards required for your stream. A shard is a unit of capacity that provides 1 MB/second of write and 2 MB/second of read throughput. This mode gives you more granular control over scaling and is suitable for applications with consistent, predictable workloads
#### Applications
Kinesis Data Streams is used for a variety of real-time data processing and analytics applications: 

- **Real-time analytics**: Analyze website clickstreams, social media feeds, and financial transactions in real-time to generate instant insights for dashboards, reporting, and dynamic pricing.
- **Internet of Things (IoT) data ingestion**: Process streaming data from connected IoT devices, such as sensors or smart appliances. This enables you to respond programmatically or send alerts when a sensor exceeds a predefined threshold.
- **Real-time application monitoring**: Ingest application logs and events to gain a real-time view of application health, which can be used to generate immediate alerts for critical issues.
- **Fraud detection**: Analyze transaction data in real-time to detect suspicious patterns and block fraudulent transactions before they happen.
#### Core concept

|Concept|Description|
|---|---|
|**Stream**|A collection of shards that capture data records.|
|**Shard**|A unit of capacity. Each shard supports **1 MB/sec write** & **2 MB/sec read** throughput.|
|**Record**|A single data item in the stream (up to 1 MB).|
|**Partition Key**|Determines **which shard** the record is stored in.|
|**Sequence Number**|Unique identifier assigned when a record is put in the stream.|
#### Shard
A **shard** is the **base unit of capacity** and **parallelism** in a Kinesis Data Stream.
each shard independently handles a slice of your data traffic.

one shard supports:
- 1 MB/sec write throughput
- 2 MB/sec read throughput        
- 5 read transactions/sec
- data retention is 24h (default) and 7 days max

| Action           | Description                                                              | Use Case                              |
| ---------------- | ------------------------------------------------------------------------ | ------------------------------------- |
| **Split Shard**  | Divides one shard into two new shards (each handles half the key range). | Increase throughput for hot shard(s). |
| **Merge Shards** | Combines two shards into one larger shard.                               | Reduce cost when traffic decreases.   |
``` bash
aws kinesis split-shard
aws kinesis merge-shards
```

Putting record into shard

``` shell
aws kinesis put-record \
  --stream-name clickstream \
  --partition-key user123 \
  --data "eyJldmVudCI6ICJjbGljayJ9"

```

`---partition-key` is id of the shard you want to forward data to.
data order is ensured in the same shard
#### Record ordering
**Within a shard**:
- Records are stored and retrieved in **strict FIFO (first-in, first-out)** order.
- Each record has a **sequence number** that preserves its order.

**Across shards**:
- There is **no ordering guarantee**.    
- Shards are processed in **parallel**, and records from different shards may arrive at the Lambda function in **any order**.
#### Retention & Replay

**Retention period:** default 24 hours (max 7 days)
You can **replay** data by re-reading older records (as long as within retention window). Great for fault recovery or reprocessing pipelines.

When a consumer reads data, it tracks an **iterator position** inside each shard:
- **TRIM_HORIZON** → start of the stream (oldest retained record)
- **LATEST** → most recent record
- **AT_TIMESTAMP** → from a specific point in time

So, to **replay**, you simply start reading again from the **TRIM_HORIZON** or a given timestamp.

```
aws kinesis get-shard-iterator \
  --stream-name my-stream \
  --shard-id shardId-000000000000 \
  --shard-iterator-type TRIM_HORIZON

aws kinesis get-records \
  --shard-iterator <your-iterator-token>
```

#### Lambda integration

**AWS Lambda** can _automatically poll_ a Kinesis stream, process records in batches, and handle failures and retries — **no servers or consumers to manage**.

How it work
1. **Data is written** to a Kinesis stream (`PutRecord` / `PutRecords`).
2. **Lambda is triggered** automatically when new records arrive.
3. **Lambda receives a batch** of records (from one or more shards).
4. **Lambda executes your handler function** and processes the records.
5. After success:
    1. Lambda **commits the checkpoint** (tracks how far it has read).
    2. If failure → records are retried until successful or sent to a **DLQ (Dead Letter Queue)** / **on-failure destination**.

Add trigger to lambda

``` bash
aws lambda create-event-source-mapping \
  --function-name my-kinesis-processor \
  --batch-size 100 \
  --starting-position LATEST \
  --event-source-arn arn:aws:kinesis:us-east-1:123456789012:stream/my-stream
```

`batch-size`: Number of records per batch (max 10,000 / 6 MB)
`starting-position`: Where to begin (`TRIM_HORIZON` | `LATEST` | `AT_TIMESTAMP`)
`event-source-arn`: ARN of the Kinesis stream

 Parallelism
 
 Each **shard** in Kinesis is processed by **exactly one concurrent Lambda instance**. If your stream has **3 shards**, Lambda can process up to **3 batches in parallel**. To increase parallelism → **increase shard count**.

#### Kinesis Client Library (KCL)
A Java-based (also available for Python, Node.js, etc.) library that:
- Handles shard discovery, load balancing, and checkpointing for you.
- **Uses DynamoDB** to track consumer state (which sequence number you last processed).

KCL automatically polls `GetRecords`, tracks checkpoints in DynamoDB, and handles redistributing shards when there is split or merge shard.


Each **Kinesis Client Library (KCL)** worker should process 1->2 shard for max effectiveness .
**KCL automatically manages shard leases** so that workers share shards evenly.

If a single worker handles **too many shards**, then:
- You lose **parallelism** — one thread becomes a bottleneck.
- A failure or slow worker will delay multiple shards.
    
If you have **too many workers for too few shards**, then:
- You waste resources (workers will sit idle).
- You’ll have extra DynamoDB lease churn.


#### CloudWatch Metric 

| Metric                               | Description                                    |
| ------------------------------------ | ---------------------------------------------- |
| `IncomingBytes`                      | Total bytes per second put to stream           |
| `ReadProvisionedThroughputExceeded`  | Consumer reading too fast                      |
| `WriteProvisionedThroughputExceeded` | Producer writing too fast                      |
| `GetRecords.IteratorAgeMilliseconds` | Age of last record processed (should stay low) |

#### Example Scenario 

|Scenario|Correct Answer|
|---|---|
|Need real-time analytics over data stream|Kinesis Data Streams + Lambda/Data Analytics|
|Need to deliver logs to S3 automatically|Kinesis Firehose|
|Need to replay or reprocess data later|Kinesis Data Streams|
|Need ordered processing|Use same partition key per entity|
|Need to scale throughput|Split shards or switch to on-demand mode|
|Consumer lag|Check `IteratorAgeMilliseconds`|
|Messages too old to read|Increase retention period|

