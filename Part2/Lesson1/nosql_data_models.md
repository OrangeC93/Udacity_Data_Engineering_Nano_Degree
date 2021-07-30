## Non Relational Database
- Need high Availability in the data: Indicates the system is always up and there is no downtime
- Have Large Amounts of Data
- Need Linear Scalability: The need to add more nodes to the system so performance will increase linearly
- Low Latency: Shorter delay before the data is transferred once the instruction for the transfer has been received.
- Need fast reads and write

Apache Cassandra:
- Open source non sql db
- Masterless architecture
- High availability
- Linearly scalable
- Used by uber, netflix, hulu, twitte, facebook, etc

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

## Cassandra Query Language CQL
Similar to SQL

## Primary Key
- The PRIMARY KEY is made up of either just the PARTITION KEY or may also include additional CLUSTERING COLUMNS, Must be unique.
  - A Simple PRIMARY KEY is just one column that is also the PARTITION KEY. A Composite PRIMARY KEY is made up of more than one column and will assist in creating a unique value and in your retrieval queries
  - The PARTITION KEY will determine the distribution of data across the system

## Clustering Columns:
The Primary key is made up of either just the partition key or with the addition of clustering columns. 

The clustering Column will determin the sort order within a Partition.
- The clustering column will sort the data in sorted ascending order
- More than one clustering column can be added (or none!)
- From there the clustering columns will sort in order of how they were added to the primary key

## Where
- Data Modeling in Apache Cassandra is query focused, and that focus needs to be on the WHERE clause
- **Failure to include a WHERE clause will result in an error**
  - The WHERE statement is allowing us to do the fast reads. With Apache Cassandra, we are talking about big data, so we are making it fast for read purposes. Data is spread across all the nodes. By using the WHERE statement, we know which node to go to, from which node to get that data and serve it back. For example, imagine we have 10 years of data on 10 nodes or servers. So 1 year's data is on a separate node. By using the WHERE year = 1 statement we know which node to visit fast to pull the data from.
- The Partition key must be included in your query and any Clustering columns can be used in the order they appear in your Primary key

Select * from table: The Where clause must be included to execute queries. It is recommended that one partition be queried at a time for performance implications. It is possible to do a select * from table if you add a configuration ALLOW FILTERING to your query. This is risky, but available if absolutely necessary.

```
For example: 
Table Name: music_libaray
column1: year
column2: artist name
column3: album name
column4: city
PRIMARY KEY(year, artist_name, album_name)

Failure: select * from music_libarary where year=1970 and location='liverpool' 
Since you cannot try to access a column or a clustering column if you have not used the other defined clustering columns.

Correct: select * from music_libarary where year=1970 and artist_name='the beatles' and album_name='let it be'


```
