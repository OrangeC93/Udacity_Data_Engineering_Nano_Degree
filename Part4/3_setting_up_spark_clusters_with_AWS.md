## From local to Standalone Mode
Overview of the Set up of a Spark Cluster
- Amazon S3 will store the dataset.
- We rent a cluster of machines, i.e., our Spark Cluster, and iti s located in AWS data centers. We rent these using AWS service called Elastic Compute Cloud (EC2).
- We log in from your local computer to this Spark cluster.
- Upon running our Spark code, the cluster will load the dataset from Amazon S3 into the cluster’s memory distributed across each machine in the cluster.

New Terms:
- Local mode: You are running a Spark program on your laptop like a single machine.
- Standalone mode: You are defining Spark Primary and Secondary to work on your (virtual) machine. You can do this on EMR or your machine. Standalone mode uses a resource manager like YARN or Mesos.


## Setup Instructions AWS
EC2 vs EMR
![image](/imgs/ec2_emr.png)

HDFS:
- HDFS (Hadoop Distributed File System) is the file system in the Hadoop ecosystem. Hadoop and Spark are two frameworks providing tools for carrying out big-data related tasks. 
- While Spark is faster than Hadoop, Spark has one drawback. It lacks a distributed storage system. In other words, Spark lacks a system to organize, store and process data files, therefore, it leverages using HDFS or AWS S3, or any other distributed storage.

MapReduce System
- HDFS uses MapReduce system as a resource manager to allow the distribution of the files across the hard drives within the cluster. Think of it as the MapReduce System storing the data back on the hard drives after completing all the tasks.

- Spark, on the other hand, runs the operations and holds the data in the RAM memory rather than the hard drives used by HDFS. Since Spark lacks a file distribution system to organize, store and process data files, Spark tools are often installed on Hadoop because Spark can then use the Hadoop Distributed File System (HDFS).


EMR Cluster
- Since a Spark cluster includes multiple machines, in order to use Spark code on each machine, we would need to download and install Spark and its dependencies. This is a manual process. Elastic Map Reduce is a service offered by AWS that negates the need.
- In order to use EMR, you need to septup AWS instructions, install and configure CLI v2, using CLI to create EMR cluster.

## Submitting Spark Scripts
The script: (make sure upload the files into the S3, and already got EMR set up)
![image](/imgs/submit_spark_scripts.png)

Execute the file using ```spark-submit <filename>.py```.

## Difference between HDFS and AWS S3
- **AWS S3** is an object **storage** system that stores the data using key value pairs, namely bucket and key, and **HDFS** is an actual **distributed file system** which guarantees fault tolerance. HDFS achieves fault tolerance by having duplicate factors, which means it will duplicate the same files at 3 different nodes across the cluster by default (it can be configured to different numbers of duplication).
- HDFS has usually been installed in **on-premise systems**, and traditionally have had engineers on-site to maintain and troubleshoot Hadoop Ecosystem, which cost more than having data on cloud. Due to the flexibility of location and **reduced cost of maintenance**, cloud solutions have been more popular. With extensive services you can use within AWS, S3 has been a more popular choice than HDFS.
- Since **AWS S3** is a binary object store, it can store **all kinds** of format, even images and videos. HDFS will strictly **require a certain file format** - the popular choices are avro and parquet, which have relatively high compression rate and which makes it useful to store large dataset.


## [Read and Writing to S3](https://github.com/udacity/nd027-c3-data-lakes-with-spark/blob/master/Setting_Spark_Cluster_In_AWS/demo_code/test-emr.ipynb)
```df = spark.read.load("s3://my_bucket/path/to/file/file.csv")```

## Read and Writing to HDFS
Log into the EMR system and upload the files in HDFS 
```hdfs -dfs -copyFromLocal sparkify_log_small.json /folder you created``` and you can browse the file on EMR GUI. 

```df = spark.read.load("hdfs:///user/folder/file.json")```
