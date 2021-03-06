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

## OLAP Cubes
An OLAP cube is an aggregation of a fact metric on a number of dimensions
- Eg Movie, Branch, Month
- Easy to communicate to business users
- Common OLAP operations include: Rollup, drill-down, slice & dice

## OLAP Cubes: Roll-Up and Drill Down
Roll-up: sum up the sales of each city by country: eg, US, France(less columns in branch dimension).

Drill-down: decompose the sales of each city into smaller districts(more columns in branch dimension)

The OLAP cubes should store the finest grain of data(atomic data), in case we need to drill down to the lowest level, eg country->city->distric->street, etc.

## OLAP Cubes query optimization
Business users will typically want to slice, dice, roll up and drill down all the time

Each such combination will potentially go through all the facts table (suboptimal)

The "GROUP by CUBE(movie, branch, month)" will make one pass through the facts table and will aggregate all possible combinations of groupings, of length 0,1,2 and 3, eg:

![image](/imgs/cube_query_optimization.png)

Saving/Materializing the output of the CUBE operation and using it is usually enough to answer all forthcoming aggregations from business users without haivng to process the whole facts table again

## OLAP Cubes Demo: Slicing & Dicing
Slicing 
- Slicing is the reduction of the dimensionality on a cube by 1 eg. 3 dimensions to 2, fixing one of the dimensions to a single values
- In the example, we have a 3 dimensional cube on day, rating and country
- In the example, rating is fixed and to "PG-13" which reduces the dimensionality

Dicing
- Creating a subcube, same dimensionality less values for 2 or more dimensions
- eg. rating to (PG-13, PG), Date to (1, 15, 30), city to (Bellevue, Lancaster)


## OLAP Cubes Demo: Roll-up
Roll-up
- Stepping up the level of aggregation to a large grouping
- eg city is summed as country

Drill-down
- Breaking up one of the dimensions to a lower level
- eg city is broken up to districts

## OLAP Cubes Demo: Grouping Sets & CUBE
Grouping sets
```
GROUP BY grouping sets((), dimDate.month, dimStore.country, (dimDate.month, dimStore.country))
```

equal to 

Cube
```
GROUP BY cube(dimDate.month, dimStore.country)
```

## Data Warehouse Technoloy

