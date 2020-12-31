
## Basic concepts
Data pipeline: 
- Load application event data from a source shuch as s3 or Kafka
- Load the data into an analytic warehouse such as Redshift
- Perform data transformations 


Extract Transform Load (ETL) and Extract Load Transform (ELT):
```
"ETL is normally a continuous, ongoing process with a well-defined workflow. ETL first extracts data from homogeneous or heterogeneous data sources. Then, data is cleansed, enriched, transformed, and stored either back in the lake or in a data warehouse.

"ELT (Extract, Load, Transform) is a variant of ETL wherein the extracted data is first loaded into the target system. Transformations are performed after the data is loaded into the data warehouse. ELT typically works well when the target system is powerful enough to handle transformations. Analytical databases like Amazon Redshift and Google BigQ."
```

Data validation in Actions
- After loading from S3 to Redshift:
  - Validate the number of rows in Redshift match the number of records in S3
- Once the location business analysis is complete:
  - Validate that all locations have a daily visit average greater than 0
  - Validate that the number of locations in our output table match the number of tables in the input table
  
  DAGs and Data Pipelines
  - DAGs are a special subset of graphs in which the edges between nodes have a specific direction, and no cycles exist.
  
## Apache Airflow
Apache Airflow is an open-source tool which structures data pipelines as DAGs.
- It allows users to write DAGs in Python that run on a schedule and/or from an external trigger. 
- Airflow is simple to maintain and can run data analysis itself, or trigger external tools (redshift, Spark, Presto, Hadoop, etc) during execution.
- It provides a web-based UI for users to visualize and interact with their data pipelines.

## How airflow works
![image](/imgs/airflow_architecture.png)
- The Airflow Scheduler starts DAGs based on time or external triggers.
- Once a DAG is started, the Scheduler looks at the steps within the DAG and determines which steps can run by looking at their dependencies.
- The Scheduler places runnable steps in the queue.
- Workers pick up those tasks and run them.
- Once the worker has finished running the step, the final status of the task is recorded and additional tasks are placed by the scheduler until all tasks are complete.
- Once all tasks have been completed, the DAG is complete.

## Building a Data Pipeline 
Creating a DAG:
```
from airflow import DAG
divvy_dag = DAG(
    'divvy',
    description='Analyzes Divvy Bikeshare Data',
    start_date=datetime(2019, 2, 4),
    schedule_interval='@daily')
```
    
Creating Operators to Perform Tasks
```
from airflow import DAG
from airflow.operators.python_operator import PythonOperator

def hello_world():
    print(“Hello World”)

divvy_dag = DAG(...)
task = PythonOperator(
    task_id=’hello_world’,
    python_callable=hello_world,
    dag=divvy_dag)
```

## Operators and Tasks
Operators define the atomic steps of work that make up a DAG. Airflow comes with many Operators that can perform common operations. Here are a handful of common ones:
- PythonOperator
- PostgresOperator
- RedshiftToS3Operator
- S3ToRedshiftOperator
- BashOperator
- SimpleHttpOperator
- Sensor

Task Dependencies:
- a.set_downstream(b) means a comes before b
- a.set_upstream(b) means a comes after b

```
hello_world_task = PythonOperator(task_id=’hello_world’, ...)
goodbye_world_task = PythonOperator(task_id=’goodbye_world’, ...)
...
hello_world_task.set_downstream(goodbye_world_task)
```
## Airflow Hooks
Connection via Airflow Hooks
- Connections can be accessed in code via hooks. Hooks provide a reusable interface to external systems and databases. With hooks, you don’t have to worry about how and where to store these connection strings and secrets in your code.
  - HttpHook
  - PostgresHook (works with RedShift)
  - MySqlHook
  - SlackHook
  - PrestoHook
```
import datetime
import logging

from airflow import DAG
from airflow.models import Variable
from airflow.operators.python_operator import PythonOperator
from airflow.hooks.S3_hook import S3Hook


def list_keys():
    hook = S3Hook(aws_conn_id='aws_credentials')
    bucket = Variable.get('s3_bucket')
    prefix = Variable.get('s3_prefix')
    logging.info(f"Listing Keys from {bucket}/{prefix}")
    keys = hook.list_keys(bucket, prefix=prefix)
    for key in keys:
        logging.info(f"- s3://{bucket}/{key}")


dag = DAG(
        'lesson1.exercise4',
        start_date=datetime.datetime.now())

list_task = PythonOperator(
    task_id="list_keys",
    python_callable=list_keys,
    dag=dag
)
```

## Context and Templating
Airflow leverages templating to allow users to “fill in the blank” with important runtime variables for tasks.


## Demo: building the S3 TO Redshift DAG
```
import datetime
import logging

from airflow import DAG
from airflow.contrib.hooks.aws_hook import AwsHook
from airflow.hooks.postgres_hook import PostgresHook
from airflow.operators.postgres_operator import PostgresOperator
from airflow.operators.python_operator import PythonOperator

import sql_statements


def load_data_to_redshift(*args, **kwargs):
    aws_hook = AwsHook("aws_credentials")
    credentials = aws_hook.get_credentials()
    redshift_hook = PostgresHook("redshift")
    redshift_hook.run(sql.COPY_ALL_TRIPS_SQL.format(credentials.access_key, credentials.secret_key))


dag = DAG(
    'lesson1.exercise6',
    start_date=datetime.datetime.now()
)

create_table = PostgresOperator(
    task_id="create_table",
    dag=dag,
    postgres_conn_id="redshift",
    sql=sql_statements.CREATE_TRIPS_TABLE_SQL
)

copy_task = PythonOperator(
    task_id='load_from_s3_to_redshift',
    dag=dag,
    python_callable=load_data_to_redshift
)

location_traffic_task = PostgresOperator(
    task_id="calculate_location_traffic",
    dag=dag,
    postgres_conn_id="redshift",
    sql=sql_statements.LOCATION_TRAFFIC_SQL
)

create_table >> copy_task
copy_task >> location_traffic_task
```
