# Query data

Querying data is the foundational step for performing nearly all data-driven tasks in Databricks. Regardless of the language or tool used, workloads start by defining a query against a table or other data source and then performing actions to gain insights from the data. This article outlines the core concepts and procedures for running queries across various Databricks product offerings, and includes code examples you can adapt for your use case.

You can query data interactively using:

* Notebooks
* SQL editor
* File editor
* Dashboards

You can also run queries as part of Delta Live Tables pipelines or workflows.

For an overview of streaming queries on Databricks, see Query streaming data.

### What data can you query with Databricks?

Databricks supports querying data in multiple formats and enterprise systems. The data you query using Databricks falls into one of two broad categories: data in a Databricks lakehouse and external data.

#### What data is in a Databricks lakehouse?

The Databricks Data Intelligence Platform stores all of your data in a Databricks lakehouse by default.

This means that when you run a basic `CREATE TABLE` statement to make a new table, you have created a lakehouse table. Lakehouse data has the following properties:

* Stored in the Delta Lake format.
* Stored in cloud object storage.
* Governed by Unity Catalog.

Most lakehouse data on Databricks is registered in Unity Catalog as managed tables. Managed tables provide the easiest syntax and behave like other tables in most relational database management systems. Managed tables are recommended for most use cases and are suitable for all users who don’t want to worry about the implementation details of data storage.

An _unmanaged table_, or _external table_, is a table registered with a `LOCATION` specified. The term _external_ can be misleading, as external Delta tables are still lakehouse data. Unmanaged tables might be preferred by users who directly access tables from other Delta reader clients. For an overview of differences in table semantics, see What is a table?.

Some legacy workloads might exclusively interact with Delta Lake data through file paths and not register tables at all. This data is still lakehouse data, but can be more difficult to discover because it’s not registered to Unity Catalog.

Note

Your workspace administrator might not have upgraded your data governance to use Unity Catalog. You can still get many of the benefits of a Databricks lakehouse without Unity Catalog, but not all functionality listed in this article or throughout the Databricks documentation is supported.

#### What data is considered external?

Any data that isn’t in a Databricks lakehouse can be considered external data. Some examples of external data include the following:

* Foreign tables registered with Lakehouse Federation.
* Tables in the Hive metastore backed by Parquet.
* External tables in Unity Catalog backed by JSON.
* CSV data stored in cloud object storage.
* Streaming data read from Kafka.

Databricks supports configuring connections to many data sources. See [Connect to data sources](broken-reference).

While you can use Unity Catalog to govern access to and define tables against data stored in multiple formats and external systems, Delta Lake is a requirement for data to be considered in the lakehouse.

Delta Lake provides all of the transactional guarantees in Databricks, which are crucial for maintaining data integrity and consistency. If you want to learn more about transactional guarantees on Databricks data and why they’re important, see What are ACID guarantees on Databricks?.

Most Databricks users query a combination of lakehouse data and external data. Connecting with external data is always the first step for data ingestion and ETL pipelines that bring data into the lakehouse. For information about ingesting data, see [Ingest data into a Databricks lakehouse](broken-reference).

### Query tables by name

For all data registered as a table, Databricks recommends querying using the table name.

If you’re using Unity Catalog, tables use a three-tier namespace with the following format: `<catalog-name>.<schema-name>.<table-name>`.

Without Unity Catalog, table identifiers use the format `<schema-name>.<table-name>`.

Note

Databricks inherits much of its SQL syntax from Apache Spark, which does not differentiate between `SCHEMA` and `DATABASE`.

Querying by table name is supported in all Databricks execution contexts and supported languages.

```
SELECT * FROM catalog_name.schema_name.table_name
```

```
spark.read.table("catalog_name.schema_name.table_name")
```

### Query data by path

You can query structured, semi-structured, and unstructured data using file paths. Most files on Databricks are backed by cloud object storage. See Work with files on Databricks.

Databricks recommends configuring all access to cloud object storage using Unity Catalog and defining volumes for object storage locations that are directly queried. Volumes provide human-readable aliases to locations and files in cloud objects storage using catalog and schema names for the filepath. See Connect to cloud object storage using Unity Catalog.

The following examples demonstrate how to use Unity Catalog volume paths to read JSON data:

```
SELECT * FROM json.`/Volumes/catalog_name/schema_name/volume_name/path/to/data`
```

```
spark.read.format("json").load("/Volumes/catalog_name/schema_name/volume_name/path/to/data")
```

For cloud locations that aren’t configured as Unity Catalog volumes, you can query data directly using URIs. You must configure access to cloud object storage to query data with URIs. See Configure access to cloud object storage for Databricks.

The following examples demonstrate how to use URIs to query JSON data in Azure Data Lake Storage Gen2, GCS, and S3:

```
SELECT * FROM json.`abfss://container-name@storage-account-name.dfs.core.windows.net/path/to/data`;

SELECT * FROM json.`gs://bucket_name/path/to/data`;

SELECT * FROM json.`s3://bucket_name/path/to/data`;
```

```
spark.read.format("json").load("abfss://container-name@storage-account-name.dfs.core.windows.net/path/to/data")

spark.read.format("json").load("gs://bucket_name/path/to/data")

spark.read.format("json").load("s3://bucket_name/path/to/data")
```

### Query data using SQL warehouses

Databricks uses SQL warehouses for compute in the following interfaces:

* SQL editor
* Databricks SQL queries
* Dashboards
* Legacy dashboards
* SQL alerts

You can optionally use SQL warehouses with the following products:

* Databricks notebooks
* Databricks file editor
* Databricks workflows

When you query data with SQL warehouses, you can use only SQL syntax. Other programming languages and APIs are not supported.

For workspaces that are enabled for Unity Catalog, SQL warehouses always use Unity Catalog to manage access to data sources.

Most queries that are run on SQL warehouses target tables. Queries that target data files should leverage Unity Catalog volumes to manage access to storage locations.

Using URIs directly in queries run on SQL warehouses can lead to unexpected errors.

### Query data using all purpose compute or jobs compute

Most queries that you run from Databricks notebooks, workflows, and the file editor run against compute clusters configured with Databricks Runtime. You can configure these clusters to run interactively or deploy them as _jobs compute_ that power workflows. Databricks recommends that you always use jobs compute for non-interactive workloads.

#### Interactive versus non-interactive workloads

Many users find it helpful to view query results while transformations are processed during development. Moving an interactive workload from all-purpose compute to jobs compute, you can save time and processing costs by removing queries that display results.

Apache Spark uses lazy code execution, meaning that results are calculated only as necessary, and multiple transformations or queries against a data source can be optimized as a single query if you don’t force results. This contrasts with the eager execution mode used in pandas, which requires calculations to be processed in order before passing results to the next method.

If your goal is to save cleaned, transformed, aggregated data as a new dataset, you should remove queries that display results from your code before scheduling it to run.

For small operations and small datasets, the time and cost savings might be marginal. Still, with large operations, substantial time can be wasted calculating and printing results to a notebook that might not be manually inspected. The same results could likely be queried from the saved output at almost no cost after storing them.
