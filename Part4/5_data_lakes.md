## Why data lakes
Data Warehouse is still the best way to go for many organizations. And we need data lake for the below reasons:
- The abundance of unstructed data (text, xml, json, logs, sensor data, imagrse, voice)
- Unprecedented data volumns (social, IoT, machine-generated, etc...)
- The rise of big data technologies like HDFS, Spark, etc...
  - The HDFS made it possible to store Petabytes of data , much lower cost per TB compared to MPP databases
  - It's possible to make data analysis without inserting into predefined schema. One can load a CSV file and make a query without creating a table, inserting the data in the table, which is known as "Schema-On-Road"
- New types of data analysis gaining momentum, e.g. predictive analytics, recommender system, graph analytics, etc...
- Emergence of new roles like data sicentist who needs freedom to represent data, join data setns together, retrieve new external data sources and more, 

Big data Tech Effect:
- Low cost -> ETL offloading
- Schema on Read
  - Schema inferred
  ![image](/imgs/schema_inferred.png)
- Un/Semi Structured support
  - Different format of files:
    - Text based many formats, csv, json, text
    - Binary formats such as Avro and Parquet
    - Compresssed formats, eg. gzip a& snappy
  - Read/write from a variety of file systems
    - Local file system
    - HDFS
    - S3
  - Read/write from a variety of databases
    - SQL throught JDBC
    - NoSql: MongoDb, Cassandra, Neo4j
  - All exposed in a single abstraction, the dataframe, and could be processed with SQL
  
## Data Lake Concepts
![image](/imgs/data_lake_structure.png)
- All types of data are welcom, high/low value, (un, semi) unstructured
- Data is stored "as-is", transformations are done later, i.e Extract-Load-Transform ELT instead of ETL
- Data is processed with schema-on-read, no predefined star-shema is there before transformation
- Massive parallelism & scalability come out of the box with all big data processing tools. Columnar Storage using parquet without expensive MPP databases
- Packages like Mlib and Graphx for Advanced Analytics such as: Machine Learning, Graph analytics & Recommendar systems are supported

## Data Lakes on AWS
![image](/imgs/data_lake_options_on_AWS.png)
- EMR (HDFS+Spark)
![image](/imgs/hdfs_spark.png)
- EMR (S3+Spark)
![image](/imgs/s3_spark.png)
- Athena
![image](/imgs/athena.png)
