

---

Use case example: tracking user activity

Imagine a mobile app that sends a stream of user interaction events (clicks, purchases, etc.) to a Kinesis data stream. The app's producer can embed the `user_id` as the partition key for every event record. 

This ensures the following:

- All events for a single user are directed to the same shard in the stream.
- A consumer application can process all the events for a particular user in the exact order they occurred, allowing it to build an accurate and real-time profile of that user's activity.
- If you wanted to calculate a "leaderboard" for the most active players, your consumer could process the data for each player from a single, dedicated shard without needing to aggregate data from multiple locations. 

---

The importance of key distribution

While embedding a primary key is useful for ordering and locality, a poor choice of key can lead to performance problems.

- **Hot shards**: If too many records have the same partition key, that single shard can become "hot" and experience an overloaded throughput. This can limit the overall scalability of the stream, as one busy shard can slow down the entire system.
- **Balancing keys**: To maximize throughput and distribute the load evenly, you want to choose a primary key with high cardinality and a good distribution of values. 

For use cases that do not require data to be grouped, a random partition key (like a UUID) can be used to ensure an even distribution of records across all shards


[https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/using-messagededuplicationid-property.html](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/using-messagededuplicationid-property.html)


