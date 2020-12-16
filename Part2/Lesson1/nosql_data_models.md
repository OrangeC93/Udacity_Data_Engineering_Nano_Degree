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


## Primary Key
- Must be unique
- The PRIMARY KEY is made up of either just the PARTITION KEY or may also include additional CLUSTERING COLUMNS
- A Simple PRIMARY KEY is just one column that is also the PARTITION KEY. A Composite PRIMARY KEY is made up of more than one column and will assist in creating a unique value and in your retrieval queries
- The PARTITION KEY will determine the distribution of data across the system

## Clustering Columns:
The Primary key is made up of either just the partition key or with the addition of clustering columns. The clustering olumn will determin the sort order within a Partition.
- The clustering column will sort the data in sorted ascending order
- More than one clustering column can be added (or none!)
- From there the clustering columns will sort in order of how they were added to the primary key

## Where
- Data Modeling in Apache Cassandra is query focused, and that focus needs to be on the WHERE clause
- Failure to include a WHERE clause will result in an error
- The Partition key must be included in your query and any Clustering columns can be used in the order they appear in your Primary key

Select * from table: The Where clause must be included to execute queries. It is recommended that one partition be queried at a time for performance implications. It is possible to do a select * from table if you add a configuration ALLOW FILTERING to your query. This is risky, but available if absolutely necessary.

```
For example: 
PRIMARY KEY(year, artist_name, album_name)
Failure: select * from music_libarary where year=1970 and location='liverpool' 
Since you cannot try to access a column or a clustering column if you have not used the other defined clustering columns.
Correct: select * from music_libarary where year=1970 and artist_name='the beatles' and album_name='let it be'


```
