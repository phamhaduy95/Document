Amazon DynamoDB Streams is a feature that ==captures a time-ordered sequence of all item-level modifications (insertions, updates, and deletions) in a DynamoDB table==. The stream records are durably stored in a log for up to 24 hours, and applications can read and process these records in near-real-time. 

Purpose

The primary purpose of DynamoDB Streams is to enable event-driven architectures and facilitate integrations with other AWS services. It allows you to automatically trigger actions and update other systems whenever data in a DynamoDB table changes, without impacting the performance of your main application. 

Common use cases include:

- **Database triggers:** By integrating with AWS Lambda, you can automatically run custom code in response to item-level changes. This can be used for tasks such as sending a welcome email to a new user after they are created in a database.
- **Real-time analytics:** Changes can be captured and pushed to an analytics service like Amazon Redshift or OpenSearch for real-time dashboards and insights.
- **Replication:** DynamoDB Global Tables, which provide multi-Region replication, are built on top of DynamoDB Streams to propagate data changes across different AWS Regions.
- **Auditing and logging:** A complete, time-ordered log of all data modifications can be captured for compliance, auditing, and debugging purposes.
- **Data synchronization:** It can be used to synchronize data between DynamoDB and other data stores, such as replicating data into a separate table or a data lake in Amazon S3. 

---

Features

- **Time-ordered sequence:** The stream records are stored in the exact order in which the item modifications occurred.
- **Near real-time processing:** Applications can read and process the changes with minimal latency.
- **24-hour retention:** The stream records are available for up to 24 hours. After this period, the data is automatically removed.
- **Seamless Lambda integration:** You can configure a Lambda function as a "trigger" to automatically process stream records. Lambda handles the stream management, error handling, and parallel processing, so you only need to focus on your business logic.
- **Integration with other services:** The stream data can also be consumed by other services, such as Amazon Kinesis Data Streams and Amazon Kinesis Data Firehose, for more advanced streaming applications.
- **Encrypted streams:** If your DynamoDB table is encrypted at rest, the corresponding stream is also encrypted. 

---

StreamViewType modes

When you enable DynamoDB Streams for a table, you can choose one of four `StreamViewType` modes to determine what information is written to the stream. This allows you to balance the verbosity of the stream data with your application's needs. 

- **`KEYS_ONLY`**: The stream record contains only the key attributes of the modified item. This is the most efficient option if you only need to know which item changed.
- **`NEW_IMAGE`**: The stream record contains the entire item as it appeared _after_ it was modified. This is useful for applications that need the latest state of an item.
- **`OLD_IMAGE`**: The stream record contains the entire item as it appeared _before_ it was modified. This is useful for applications that need to compare the old state of an item to the new one.
- **`NEW_AND_OLD_IMAGES`**: The stream record contains both the `NEW_IMAGE` and the `OLD_IMAGE`. This is the most detailed option and is required for some features, such as DynamoDB Global Tables