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

