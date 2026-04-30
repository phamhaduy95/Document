#### Dead-letter queue

In Amazon Simple Queue Service (SQS), a Dead Letter Queue (DLQ) is a special type of SQS queue used to store messages that cannot be processed successfully by a consumer after a specified number of attempts.

**Key Purposes of a DLQ**
- **Isolate Failed Messages**: Move unsuccessful messages out of the main queue to prevent them from blocking other messages or causing repeated failures.
- **Debugging and Analysis**: Store failed messages for later inspection to identify and fix issues in the consumer application or message data.
- **Prevent Message Loss**: Ensure problematic messages are retained rather than discarded, allowing for manual intervention or reprocessing.
- **Improve System Reliability**: Avoid infinite retry loops that could degrade application performance or increase costs.

A **Dead-Letter Queue** stores messages that fail processing repeatedly.

You configure a DLQ with a `MaximumReceiveCount`, e.g., 5.  
After 5 failed receives, the message moves automatically to the DLQ.