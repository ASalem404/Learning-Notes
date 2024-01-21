- DynamoDB:

  - What is DynamoDB ?

    - Scalability: DynamoDB is designed for seamless horizontal scaling.
      As your application grows, you can easily increase the read and write capacity to accommodate higher traffic.

    - Performance: DynamoDB offers low-latency access to data with consistent and predictable performance,
      making it suitable for real-time applications.

  - Primary Key:

    - consist of unique partition key elements or a unique combination of partition and sort keys
    - Dynamo uses the partition key to create the hash table and seperate data into partitions and uses this
      key to know which partition to use for your queries.
      ![partitions example](partitions.png)

    - Sort Key (Range Key): The sort key is optional, and it is used to organize items within a partition.
      It enables efficient querying and sorting of data within a partition.
      Items with the same partition key but different sort keys are ordered based on the sort key.
      A sort key value is sometimes referred to as a range key.

  - DAX:

    - Amazon DynamoDB Accelerator (DAX) is a fully managed, highly available caching service built for Amazon DynamoDB.
      DAX delivers up to a 10 times performance improvement—from milliseconds to microseconds—even at millions of requests per second.
    - DAX does all the heavy lifting required to add in-memory acceleration to your DynamoDB tables,
      without requiring developers to manage cache invalidation, data population, or cluster management.

  - GSI:

    - Global Secondary Index is an index with a partition key and an optional sort key that is different from the primary key of the table.
      This allows you to query and retrieve data from DynamoDB using non-primary key attributes.
    - GSIs are useful when you need to perform queries that are not efficiently supported by the primary key of the table.
    - You can create multiple GSIs on a single table to support different query patterns.
    - ike the main table, a GSI has its own provisioned throughput (read and write capacity units).
      You can adjust the provisioned throughput for GSIs independently of the main table.
    - The cost is based on the provisioned throughput capacity, regardless of whether you fully utilize it or not.
      You can independently provision read and write capacity for each GSI based on your application's needs.
      In addition to provisioned throughput costs, you are also charged for the storage of data in the GSI.
