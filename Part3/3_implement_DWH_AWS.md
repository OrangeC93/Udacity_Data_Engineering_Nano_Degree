## Choices for Implementing a Data Warehouse
![image](/imgs/cloud.png)
![image](/imgs/premise.png)

## DWH Dimensional Model Storage on AWS
![image](/imgs/dimensional_model_storage_on_asw.png)
![image](/imgs/dimensional_model_storage_on_asw2.png)

## Amazon Redshift Technoloy
Normal relational databases
- Execute multiple queries in parallel if they have access to many cores/servers. However, every query is always executed on a single CPU of a single machine
- Acceptable for OLTP, mostly updates and few rows retrieval
![image](/imgs/normal_relational_databases.png)

Amazon Redshift
- Massively Parallel Processing (MPP) databases parallelize the execution of one query on mulitple CPUs/machines
- How? A table is partitioned and partitions are processed in parallel
- Amazon Redshift is a cloud-managed, column-oriented, MPP databases
![image](/imgs/mpp.png)

## Redshift Architecture: The Cluster
![image](/imgs/redshift_cluster.png)
Leader Node:
- Coordinates compute nodes
- Handles external communication
- Optimizes query execution

Compute Nodes:
- Each with own CPU, memory, and disk (determined by the node type)
- Scale up: get more powerful nodes
- Scale out: get more nodes

Node Slices:
- Each compute node is logically divided into a number of slices
- A cluster with n slices, can proces n partitions of a table simultaneously

## Redshift Architecture: Example
Examples:
![image](/imgs/redshift_cluster_eg.png)
Building a redshit cluster
- It's advisable to setup a billing alarm to monitor AWS charges
- Redshit nodes are somewhat expensive, and we spin a number of them

## SQL-To-SQL ETL, how? Different database server
Scenario 1: to copy the results of a query to another table (eg. facts or dimension table) in the same database, we can easily use SELECT INTO
```
SELECT fact1, fact2
INTO newFactTable
FROM table X, Y
WHERE X.id = Y.fid x.v<> null
GROUP BY Y.d
```

Scenario 2: to copy the results of query to another table on a totally different database server
- If both servers are running the same RDBMS, that might be possible, but harder between two completely different RDBMSs.
- And even if we can, we probably need to do some transformations, cleaning, governance, etc.
```
SELECT fact1, fact2
INTO OtherServer.newFactTable
FROM table X, Y
WHERE X.id = Y.fid x.v<> null
GROUP BY Y.d
```
## SQL-To-SQL ETL, AWS Case
![image](/imgs/etl_aws_case.png)
The advantage of using S3 for ETL storage compared to storing the data in our own EC2 instance
- S3 is AWS-managed, we dont'need to worry about storage reliability
- By using S3, we only pay for the storage we use
- By using S3, we don't need to worry about not having enough storage

## Redshift & ETL in Context
![image](/imgs/etl_in_context.png)
We need to copy the data already stored in S3 to another S3 staging bucket during the ETL process because it would be transformed before insertion into the DWH.

## Ingesting at Scale
Ingesting at Scale: Use copy
- To transfer data from an S3 staging area to redshift use the Copy command
- Inserting data row by using INSERT will be very slow
- If the file is large:
  - It's better to break it up to multiple files
  - Ingest in Parallel
    - commond prefix
    - manifest file
- Ohter considerations 
  - Better to ingest from the same AWS region
  - Better to compress all the  csv files
- One can also specify the delimiter to be used

## Redshift ETL Examples
Common prefix example: common prefix
```
s3://mySource/sales-day1.csv.gz
s3://mySource/sales-day2.csv.gz
```
![image](/imgs/common_prefix_example.png)

Manifest file example: common suffix
```
s3://mySource/day1-sales.csv.gz
s3://mySource/day2-sales.csv.gz
```
![image](/imgs/manifest_file_example.png)


## Redshift ETL Continued: 
Compression Optimization:
- The optimal compression strategy for each column type is different
- Redshift gives the user control over the compression of each column
- The COPY command makes automatic best effort compression decisions for each column

ETL from other sources
- It is also possible to ingest directly using ssh from EC2 machines
- Other than that
  - S3 needs to be used as a staging area
  - Usually, an EC2 ETL workder needs to run the ingestion jobs orchestrated by a dataflow product like Airflow, Luigi, Nifi, StreamSet, or AWS Data Pipeline
  
ETL Out of Redshift
- Redshift is accessible, like any relational database, as a JDBC/ODBC source
  - Natrually used by BI apps
- However, we many need to extract data out of Redshift to pre-aggregated OLAP CUBES
![image](/imgs/etl_out_of_redshift.png)

## Redshift Cluster Quick Launcher

## Problems with the Quick Launcher
The cluster created by the Quick Launcher is a fully-functional one, but we need more functionality...

Security:
- The cluster is accessible only from the virtual private cloud
- We need to access it from our jupyter workspace

Access to S3
- The cluster needs to access an S3 bucket

## Infrastructure as Code on AWS
Infracture-as-Code(IaC):
- The ability to create infrastructure, i.e. machines, users, roles, folders and processes using code.
- The ability to automate, maintain, deploy, replicate and share complex infractures as easily as you maintain code (undreamt-of in an on premise deployment).

How to achieve IaC on AWS
- aws-cli scripts: 
  - similar to bash scripts
  - simple & convenient
![image](/imgs/aws_cli.png)
- AWS sdk: 
  - available in lots programming languages
  - more power, could be integrated with apps
![image](/imgs/aws_sdk.png)
- Amazon Cloud formation: 
  - json description of all resources, permission, constraints
  - atomic, either all succeed or all fail
  
 ## Enabling Programmatic Access to IaC
 - Use the python AWS SDK akda boto3
 - Create one IAM user 
 - Give admin privileges (attach policy)
 - Open an incoming TCP port to access the endpoint
 - Use its access token and secret to build our cluster and configure it, that should be our last click and fill process
 
 ## Parallel ETL
 - Get the params of the created redshift cluster
  - Redshift cluster endpoint
  - IAM role ARN that give access to redshit to read from S3
 - Connect to the Redshift Cluster
 - Create Tables for partitioned data
 ![image](/imgs/creat_table1.png)
 - Load partitioned data into the cluster
![image](/imgs/load_data1.png)
 - Create Tables for the non-partitioned data
![image](/imgs/create_table2.png)
 - Load non-partitioned data into the cluster
![image](/imgs/load_data2.png)

## Optimizing Table Design
When a table is partitioned up into many pieces and distributed across slices in different machines, this is done blindly. If one has an idea about the frequency access pattern of a table, one can choose a more clever strategy.

The 2 possible strategies are:
- Distribution style
- Storting key

## Distribution Style: Even
- Round-robin over all slices to achieve load-balancing
- Good if a table won't be joined: high cost of join with even distribution(Shuffling)
![image](/imgs/even_style.png)

## Distribution Style: All
- Small tables could be replicated on all slices to speed up joins:
  - Store dimension with all distribution joins factSales with even distribution, join result in parallel, no shuffling!
- Used frequently for dimension tables
- AKR broadcasting
![image](/imgs/all_style.png)

## Distribution Style: Auto
- Leave decision to Redshit
- "Small enough" tables are distributed with an all strategy
- Large tables are distribtued with EVEN strategy

## Distribution Style: Key
- Rows havign similar values are placed in the same slice
- This can lead to a skewed distribution if some values of the dist key are more frequent than other
- However, very useful when a dimension table is too big to be distributed with ALL strategy. In that case, we distribute both the fact table and the dimension table using the same dist key
- If two tables are distributed on the joining keys, redshift collocates the rows from both tables on the same slices, which elimiates the shuffling a lot!
![image](/imgs/key_style.png)

## Sorting Key
- One can define its columns as sort key
- Upon loading, rows are sorted before distribution to slices
- Minimizes the query time since each node already has contiguous ranges of rows based on the sortign key
- Useful for columns that are used frequently in sorting like the date dimension and its corresponding foreign key in the fact table.
