# Transform data

This article provides an introduction and overview of transforming data with Databricks. Transforming data, or preparing data, is key step in all data engineering, analytics, and ML workloads.

The example patterns and recommendations in this article focus on working with lakehouse tables, which are backed by Delta Lake. Because Delta Lake provides the ACID guarantees of a Databricks lakehouse, you might observe different behavior when working with data in other formats or data systems.

Databricks recommends ingesting data into a lakehouse in a raw or nearly raw state, and then applying transformations and enrichment as a separate processing step. This pattern is known as the medallion architecture. See What is the medallion lakehouse architecture?.

If you know that the data you need to transform has not yet been loaded into a lakehouse, see [Ingest data into a Databricks lakehouse](broken-reference). If you’re trying to find lakehouse data to write transformations against, see [Discover data](broken-reference).

All transformations begin by writing either a batch or streaming query against a data source. If you’re not familiar with querying data, see [Query data](broken-reference).

Once you’ve saved transformed data to a Delta table, you can use that table as a feature table for ML. See What is a feature store?.

### Spark transformations vs. lakehouse transformations

This article focuses on defining _tranformations_ as they relate to the **T** in ETL or ELT. The Apache Spark processing model also uses the word _transformation_ in a related way. Briefly: in Apache Spark, all operations are defined as either transformations or actions.

* **Transformations**: add some processing logic to the plan. Examples include reading data, joins, aggregations, and type casting.
* **Actions**: trigger processing logic to evaluate and output a result. Examples include writes, displaying or previewing results, manual caching, or getting the count of rows.

Apache Spark uses a _lazy execution_ model, meaning that none of the logic defined by a collection of operations are evaluated until an action is triggered. This model has an important ramification when defining data processing pipelines: only use actions to save results back to a target table.

Because actions represent a processing bottleneck for optimizing logic, Databricks has added numerous optimizations on top of those already present in Apache Spark to ensure optimal execution of logic. These optimizations consider all transformations triggered by a given action at once and find the optimal plan based on the physical layout of the data. Manually caching data or returning preview results in production pipelines can interrupt these optimizations and lead to significant increases in cost and latency.

Therefore we can define a _lakehouse transformation_ to be any collection of operations applied to one or more lakehouse tables that result in a new lakehouse table. Note that while transformations such as joins and aggregations are discussed separately, you can combine many of these patterns in a single processing step and trust the optimizers on Databricks to find the most efficient plan.

### What are the differences between streaming and batch processing?

While streaming and batch processing use much of the same syntax on Databricks, each have their own specific semantics.

Batch processing allows you to define explicit instructions to process a fixed amount of static, non-changing data as a single operation.

Stream processing allows you to define a query against an unbounded, continuously growing dataset and then process data in small, incremental batches.

Batch operations on Databricks use Spark SQL or DataFrames, while stream processing leverages Structured Streaming.

You can differentiate batch Apache Spark commands from Structured Streaming by looking at read and write operations, as shown in the following table:

|           | Apache Spark         | Structured Streaming        |
| --------- | -------------------- | --------------------------- |
| **Read**  | `spark.read.load()`  | `spark.readStream.load()`   |
| **Write** | `spark.write.save()` | `spark.writeStream.start()` |

Materialized views generally conform to batch processing guarantees, although Delta Live Tables is used to calculate results incrementally when possible. The results returned by a materialized view are always equivalent to batch evaluation of logic, but Databricks seeks to process these results incrementally when possible.

Streaming tables always calculate results incrementally. Because many streaming data sources only retain records for a period of hours or days, the processing model used by streaming tables assumes that each batch of records from a data source is only processed once.

Databricks supports using SQL to write streaming queries in the following use cases:

* Defining streaming tables in Unity Catalog using Databricks SQL.
* Defining source code for Delta Live Tables pipelines.

Note

You can also declare streaming tables in Delta Live Tables using Python Structured Streaming code.

### Batch transformations

Batch transformations operate on a well-defined set of data assets at a specific point in time. Batch transformations might be one-time operations, but often are part of scheduled workflows or pipelines that run regularly to keep production systems up to date.

### Real-time transformations

Delta Lake excels at providing near real-time access to large amounts of data for all users and applications querying your lakehouse, but because of the overhead with writing files and metadata to cloud object storage, true real-time latency cannot be reached for many workloads that write to Delta Lake sinks.

For extremely low-latency streaming applications, Databricks recommends choosing source and sink systems designed for real-time workloads such as Kafka. You can use Databricks to enrich data, including aggregations, joins across streams, and joining streaming data with slowly changing dimension data stored in the lakehouse.
