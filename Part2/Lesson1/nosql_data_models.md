## Distributed Databases
In a distributed database, in order to have high availability, you will need copies of your data.

Eventual consistency: a consistency model used in distributed computing to achieve high availability that informally guarantees that, if no new updates are made to a given data item, eventually all accesses to that item will return the last updated value
## CAP Theorem
- Consistency: Every read from the database gets the latest (and correct) piece of data or an error.
- Availability: Every request is received and a response is given -- without a guarantee that the data is the latest update.
- Partition Tolerance: The system continues to work regardless of losing network connectivity between nodes.

## Denormalization in Apache Cassandra
- Denormalization is not just okay -- it's a must
- Denormalization must be done for fast reads
- Apache Cassandra has been optimized for fast writes
- ALWAYS think Queries first
- One table per query is a great strategy
- Apache Cassandra does not allow for JOINs between tables
