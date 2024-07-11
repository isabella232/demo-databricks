# Ingest data into a Databricks lakehouse

Databricks offers a variety of ways to help you ingest data into a lakehouse backed by Delta Lake. Databricks recommends using Auto Loader for incremental data ingestion from cloud object storage. The add data UI provides a number of options for quickly uploading local files or connecting to external data sources.

### Auto Loader

Auto Loader incrementally and efficiently processes new data files as they arrive in cloud storage without additional setup. Auto Loader provides a Structured Streaming source called `cloudFiles`. Given an input directory path on the cloud file storage, the `cloudFiles` source automatically processes new files as they arrive, with the option of also processing existing files in that directory.

### Upload local data files or connect external data sources

You can securely upload local data files or ingest data from external sources to create tables. See Load data using the add data UI.

### COPY INTO

COPY INTO allows SQL users to idempotently and incrementally ingest data from cloud object storage into Delta tables. It can be used in Databricks SQL, notebooks, and Databricks Jobs.

### When to use COPY INTO and when to use Auto Loader

Here are a few things to consider when choosing between Auto Loader and `COPY INTO`:

* If you’re going to ingest files in the order of thousands, you can use `COPY INTO`. If you are expecting files in the order of millions or more over time, use Auto Loader. Auto Loader requires fewer total operations to discover files compared to `COPY INTO` and can split the processing into multiple batches, meaning that Auto Loader is less expensive and more efficient at scale.
* If your data schema is going to evolve frequently, Auto Loader provides better primitives around schema inference and evolution. See Configure schema inference and evolution in Auto Loader for more details.
* Loading a subset of re-uploaded files can be a bit easier to manage with `COPY INTO`. With Auto Loader, it’s harder to reprocess a select subset of files. However, you can use `COPY INTO` to reload the subset of files while an Auto Loader stream is running simultaneously.
* For an even more scalable and robust file ingestion experience, Auto Loader enables SQL users to leverage streaming tables. See Load data using streaming tables in Databricks SQL.

For a brief overview and demonstration of Auto Loader, as well as `COPY INTO`, watch the following YouTube video (2 minutes).

\&nbsp;

### Migrate data applications to Databricks

Migrate existing data applications to Databricks so you can work with data from many source systems on a single platform. See Migrate data applications to Databricks.
