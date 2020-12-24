## Functional Programming
- Written in Scala
- Application Programming interfaces in Java, R, Python
![image](/imgs/functional_procedure_program.png)

## [Pure Function](https://www.jianshu.com/p/879f26deb16d)

## Maps and lambda functions
To get Spark to actually run the map step, you need to use an "action". One available action is the collect method. The collect() method takes the results from all of the clusters and "collects" them into a single list on the master node.

```
distributed_song_log.map(convert_song_to_lowercase).collect()
```

## Distributed Data Stores
- HDFS
![image](/imgs/distributed_data_stores.png)
- AWS

## SparkSession
![image](/imgs/spark_conf.png)
![image](/imgs/spark_session.png)

## Read and Write Data into Spark Data Frames

```
import pyspark
from pyspark import SparkConf
from pyspark.sql import SparkSession

spark = SparkSession \
    .builder \
    .appName("Our first Python Spark SQL example") \
    .getOrCreate()
    
spark.sparkContext.getConf().getAll()

path = "data/sparkify_log_small.json"
user_log = spark.read.json(path)
user_log.printSchema()
user_log.describe()
user_log.show(n=1)
user_log.take(5)

out_path = "data/sparkify_log_small.csv"
user_log.write.save(out_path, format="csv", header=True)


## Imperative vs Declarative programming
```
