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

## Kimball's Bus Architecture
![image](/imgs/kimball.png)
- Results in a common dimension data model shared by different departments
- Data is not kept at the aggregated level, but rather at the atomic level
- Organized by business processes, and used by different department

ETL: A closer look
- Extracting:
  - Get the data from its source
  - Possibly deleting old state
- Transforming:
  - Integrates many sources together
  - Possibly cleansing: inconsistencies, duplication, missing values
  - Possibly producing diagnostic metadata
- Loading:
  - Structuring and loading the data into the dimensional data model

## Independent Data Mart
![image](/imgs/data_mart.png)
- Departments have independent ETL processes & dimensional models
- These separate & smaller dimensional models are called "Data Marts"
- Different fact tables for the same events, no conformed dimensions
- Uncoordinated efforts can lead to inconsistent views
- Despite awareness of the emergence of this architecture from departmental autonomy, it is generlly **discouraged**

## CIF (Inmon's Corporate Information Factory)
![image](/imgs/CIF.png)
2 ETL Process
- Source systems -> 3NF DB
- 3NF DB -> Departmental Data Marts

The 3NF DB acts an enterprise wide data store
- Single integrated source of truth for data marts
- Could be accessed by end-users if needed

Data marts dimensionally modelled & unlike Kimball's dimensional models, they are mostly aggregated

## Hybrid Kimball Bus && Inmon CIF
![image](/imgs/hybrid.png)
The Hybrid Kimball Bus and Inmon CIF model doesn't focus on Data Marts allowing department to individualize the data ETL process and denormalized data tables.
