#### Message Polling

Các consumer nhận và sử lý message từ SQS queue thông qua cơ chế polling. SQS Polling tồn tại 2 dạng short polling (default) và long polling.

##### Short polling
- **Behavior**: SQS immediately return data whenever `ReceiveMessage` API is called, even if no messages are available. It queries only a subset of the queue’s servers (based on SQS’s distributed architecture) and may return an empty response if no messages are found on those servers.
- **Characteristics**:
  - Fast response time, even if no messages are available.
  - May result in frequent empty responses, increasing API calls and costs.
  - Suitable for low-latency applications where consumers poll infrequently or expect messages to arrive sporadically.
- **Use Case**: Scenarios where immediate response is prioritized over efficiency, or when message volume is low.
##### Long Polling
- **Behavior**: SQS waits up to a specified duration (up to 20 seconds) for messages to arrive in the queue before returning a response. If messages arrive during this period, they are returned immediately; otherwise, the response is sent after the wait time expires (potentially empty).
- **Characteristics**:
  - Reduces the number of empty responses, lowering API call costs.
  - Increases efficiency by waiting for messages to arrive, especially in queues with intermittent message traffic.
  - Slightly increases latency due to the wait time but improves throughput for busy queues.
- **Use Case**: Applications with variable message arrival rates, high-throughput workloads, or cost-sensitive environments.

`ReceiveMessageWaitTimeSeconds`
If you enable **long polling**, SQS will _wait up to a specified number of seconds_ (max = 20 seconds) for a message to arrive **before returning an empty response**.