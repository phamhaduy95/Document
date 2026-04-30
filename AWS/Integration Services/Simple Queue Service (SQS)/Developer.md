#### Message Retention time

| Setting     | Duration                    |
| ----------- | --------------------------- |
| **Default** | 4 days                      |
| **Minimum** | 60 seconds (1 minute)       |
| **Maximum** | 1,209,600 seconds (14 days) |
#### Essential Parameter

| Parameter                           | Description                                         | Typical Value                    |
| ----------------------------------- | --------------------------------------------------- | -------------------------------- |
| **Visibility Timeout**              | How long a message stays invisible after being read | 30 sec (default), up to 12 hours |
| **Message Retention Period**        | How long messages are stored if not deleted         | 4 days (default), up to 14 days  |
| **Maximum Message Size**            | Max payload                                         | 256 KB                           |
| **Delivery Delay**                  | Delay new messages from being visible               | up to 15 minutes                 |
| **Receive Message WaitTimeSeconds** | Enable long polling                                 | 0–20 seconds                     |
| **Batch Size**                      | Receive up to 10 messages per API call              | up to 10                         |

#### Delay Queues & Message Timers
- **Delay Queue** → delays _all_ new messages from being visible (configured at queue level).
- **Per-message Delay** → delays a specific message (using `DelaySeconds` parameter).

``` bash
aws sqs send-message \
  --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/myqueue \
  --message-body "Process after 10 seconds" \
  --delay-seconds 10
```

#### FIFO Queue: Deduplication

Prevents processing the same message twice within a short period.

| Mechanism                       | Purpose                                                      |
| ------------------------------- | ------------------------------------------------------------ |
| **Content-based deduplication** | Use message body hash as deduplication ID.                   |
| **Deduplication ID**            | Sender can explicitly set an ID (dedup within 5-min window). |

``` bash
aws sqs send-message \
  --queue-url https://sqs.<region>.amazonaws.com/<account-id>/<queue-name> \
  --message-body "Hello from SQS!"
  --message-deduplication-id 12323
```

#### FIFO Queue Group Order

Để đảm bảo chỉ ordering theo 1 group nhất định. Ta có thể thêm field --message-group-id trong API `SendMessage`

#### `ReceiveMessage`

```bash 
aws sqs receive-message \
  --queue-url https://sqs.<region>.amazonaws.com/<account-id>/<queue-name>
```

|Parameter|Required|Description|
|---|---|---|
|`--queue-url`|✅|The **URL** of the SQS queue to read messages from.|
|`--max-number-of-messages`|❌|The **number of messages** to return (1–10). Default is 1.|
|`--visibility-timeout`|❌|Temporarily hides messages from other consumers after being read (in seconds).|
|`--wait-time-seconds`|❌|Enables **long polling** (0–20 seconds). Reduces empty responses.|
|`--message-attribute-names`|❌|Retrieves custom **message attributes**.|
|`--attribute-names`|❌|Retrieves system attributes (like SentTimestamp, ApproximateReceiveCount).|
|`--receive-request-attempt-id`|🔸|Used in FIFO queues to avoid duplicate deliveries.|

#### Batch Sending and Receiving Message 