## Debug is Hard
- Due to the **standalone mode**, debugging in Spark is hard. 
- Spark wait for a long time before running the code **(lazy evaludation)**, so you dont descover a bug right away.
- Cannot use print statment in the cluster. Spark has a driver node coordinating the tasks of various worker nodes. Code is running on the worker nodes and not the driver, so print statements will only urn on the woker nodes, we cannot directly see the output from them (we're not connected directly to them). The original variables stored on the driver remain unchanged, making them useless for debugging

## Method: Accumulators
As the name hints, accumulators are variables that accumulate. Because Spark runs in distributed mode, the workers are running in parallel, but asynchronously. For example, worker 1 will not be able to know how far worker 2 and worker 3 are done with their tasks. With the same analogy, the variables that are local to workers are not going to be shared to another worker unless you accumulate them. **Accumulators are used for mostly sum operations, like in Hadoop MapReduce, but you can implement it to do otherwise.** Be carefull of using it since it may double count or multiple count on our counter.

## Broadcast
Spark Broadcast variables are secured, read-only variables that get distributed and cached to worker nodes. This is helpful to Spark because when the driver sends packets of information to worker nodes, it sends the data and tasks attached together which could be a little heavier on the network side. Broadcast variables seek to reduce network overhead and to reduce communications. Spark Broadcast variables are used only with Spark Context.

## Different types of Spark Functions
- Transformation
- Actions

Spark uses **lazy evaluation** to evaluate RDD and dataframe. Lazy evaluation means the code is not executed until it is needed. The **action** functions trigger the lazily evaluated functions.

```
df = spark.read.load("some csv file")
df1 = df.select("some column").filter("some condition")
df1.write("to path")
```

- If you execute this code line by line, the second line will be loaded, but you will not see the function being executed in your Spark UI.
- When you actually execute using action write, then you will see your Spark program being executed.

## Spark Web UI
Web UI is an EKG machine that helps you measur the Spark jobs, its' a very useful tool for diagnosing issues in your cluster and code.

![image](/imgs/spark_ports.png)

## Optimizing for data sknewness
1. Use Alternate Columns that are more normally distributed:
E.g., Instead of the year column, we can use Issue_Date column that isn’t skewed.

2. Make Composite Keys:
For e.g., you can make composite keys by combining two columns so that the new column can be used as a composite key. For e.g, combining the Issue_Date and State columns to make a new composite key titled Issue_Date + State. The new column will now include data from 2 columns, e.g., 2017-04-15-NY. This column can be used to partition the data, create more normally distributed datasets (e.g., distribution of parking violations on 2017-04-15 would now be more spread out across states, and this can now help address skewness in the data.

**3. Partition by number of Spark workers:**
Another easy way is using the Spark workers. If you know the number of your workers for Spark, then you can easily partition the data by the number of workers df.repartition(number_of_workers) to repartition your data evenly across your workers. For example, if you have 8 workers, then you should do df.repartition(8) before doing any operations.
