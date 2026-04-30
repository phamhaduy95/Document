
ynamoDB automatically **divides a table into multiple partitions** (also called **physical partitions**) to handle large datasets and high throughput.

Each **partition** stores a portion of your items, determined by the **hash (partition) key** value.

👉 The **partition key** is hashed using an internal algorithm, and items are distributed roughly evenly across partitions.

|Table|Total RCUs|Number of Physical Partitions|RCUs per Partition|
|---|---|---|---|
|`Orders`|3,000|3|1,000 each|


RCUs are evenly split across physical partitions.  
Each partition key value gets access to the RCUs of its **assigned physical partition only**, so uneven key access can cause throttling even if total RCUs aren’t maxed out.