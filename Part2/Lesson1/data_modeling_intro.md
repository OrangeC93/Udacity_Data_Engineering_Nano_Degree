## Data modeling
- Process to support business and user applications
- Gather requirements
- Conceptual data modeling
- Logical data modeling
- Physical data modeling

## Key points 
- Data Organization: The organization of the data for your applications is extremely important and makes everyone's life easier.
- Use cases: Having a well thought out and organized data model is critical to how that data can later be used. Queries that could have been straightforward and simple might become complicated queries if data modeling isn't well thought out.
- Starting early: Thinking and planning ahead will help you be successful. This is not something you want to leave until the last minute.
- Iterative Process: Data modeling is not a fixed process. It is iterative as new requirements and data are introduced. Having flexibility will help as new information becomes available.

## Relational database
![image](/imgs/relational_database.png)

## When to use Relational databse
SQL, easier to change business requirements, modeling the data, secondary indexes available, ACID transactions -- data integrity

## ACID transactions
- Atomicity: The whole transaction is processed or nothing is processed. 
- Consistency: Only transactions that abide by constraints and rules are written into the database, otherwise the database keeps the previous state. The data should be correct across all rows and tables.
- Isolation: Transactions are processed independently and securely, order does not matter. A low level of isolation enables many users to access the data simultaneously, however this also increases the possibilities of concurrency effects (e.g., dirty reads or lost updates). On the other hand, a high level of isolation reduces these chances of concurrency effects, but also uses more system resources and transactions blocking each other. 
- Durability: Completed transactions are saved to database even in cases of system failure. A commonly cited example includes tracking flight seat bookings. So once the flight booking records a confirmed seat booking, the seat remains booked even if a system failure occurs. 

## Creating Tables in Postgres

------------
## NoSQL database
NoSQL = NOT only SQL, NoSQL and NonRelational are interchangeable terms

Common Types of NoSQL database
- Apache Cassandra (Partition Row store)
- MongoDB (Document store)
- DynamoDB (Key-Value store)
- Apache HBase (Wide Column store)
- Neo4J (Graph Database)

The Basic:
- Keyspace = collection of tables
- Table = a group of partitions
- Rows = a single item
- Partition
- Primary key
- Columns

## When to use NoSQL Database
