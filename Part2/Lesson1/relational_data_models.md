## OLAP vs OLTP
Online Analytical Processing (OLAP):
- Databases optimized for these workloads allow for complex analytical and ad hoc queries, including aggregations. These type of databases are optimized for reads.

Online Transactional Processing (OLTP):
- Databases optimized for these workloads allow for less complex queries in large volume. The types of queries for these databases are read, insert, update, and delete.

The key to remember the difference between OLAP and OLTP is analytics (A) vs transactions (T). If you want to get the price of a shoe then you are using OLTP (this has very little or no aggregations). If you want to know the total stock of shoes a particular store sold, then this requires using OLAP (since this will require aggregations).

## Structuring the Database: Normalization
Normalization: to reduce daeta redundancy and increase data integrity.

Denormalization: must be done in read heavy workloads to increase performance.

## Normal Forms
How to reach First Normal Form (1NF):
- Atomic values: each cell contains unique and single values

Second Normal Form (2NF):
- Have reached 1NF
- All columns in the table must rely on the Primary Key

Third Normal Form (3NF):
- Must be in 2nd Normal Form
- No transitive dependencies

## Denormalization:
JOINS on the database allow for outstanding flexibility but are extremely slow. If you are dealing with heavy reads on your database, you may want to think about denormalizing your tables. You get your data into normalized form, and then you proceed with denormalization. So, denormalization comes after normalization.

Logical Design Change:
- The designer is in charge of keeping data consistent
- Reads will be faster (select)
- Writes will be slower (insert, update, delete)

## Fact and Dimension Tables
- Work together to create and organizaed data mode
- While fact and dimension are not created differently in DDL, they are conceptual and extremely important for organization.

Fact tables consists of the measurements, metrics or facts of a business process.

Dimension: a structure that categorizes facts and measures in order to enable users to answer business questions. Dimensions are people, products, place and time.

## Star Schema
