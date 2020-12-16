## Operational vs Analytical business processes
Operational Processes:
- Find goods & make orders
- Stock and find goods
- Pick up & deliver goods

Analytical Processes:
- Assess the performance of sales staff
- See the effect of different sales channels 
- Monitor sales growth 

Operational database:
- Excellent for operations
- No redundancy, high integrity
- Too slow for analytics, too many joins
- Too hard to understand

Solution: create 2 processing modes, create a system for them to co-exist
- OLTP: online transactional processing
- OLAP: online analytical processing
- Data Warehouse is system (inclusing processes, technologies and data representations) that enable us to support analytical processes

## Data Warehouse: Technical Perspective
ETL: Extract the data from the source systems used for operations, Transform the data and load it into a dimensional model.

Dimensional model: Designed to (1) make it easy for business users to work with the data (2) improve analytical queries performance. The technologies used for storing dimensional models are different than traditional technologies.

Business-user-facing application ae needed, with clear visuals.

## Dimensional Modeling
Dimensional modelling goals: (1) easy to understand (2) fast analytical query performance
- Star Schema: joins with dimensions only good for OLAP not OLTP
- 3NF: lots of expensive joins, hard to explain to busness users
Facts & Dimensions:
- Fact tables: metrics like quantity of an item, duration of a call, a book rating
- Dimension tables: attributes like the sore at which an item is purchased, or the customer who made the call

Fact or Dimension Dilemma: for facts, if you're unsure if a column is a fact or dimension, the simplest rule is that a fact is usually: Numberic & Additive
