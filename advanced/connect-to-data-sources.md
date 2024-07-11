# Connect to data sources

This article provides opinionated recommendations for how administrators and other power users can configure connections between Databricks and data sources. If you are trying to determine whether you have access to read data from an external system, start by reviewing the data that you have access to in your workspace. See [Discover data](broken-reference).

You can connect your Databricks account to data sources such as cloud object storage, relational database management systems, streaming data services, and enterprise platforms such as CRMs. The specific privileges required to configure connections depends on the data source, how permissions in your Databricks workspace are configured, the required permissions for interacting with data in the source, your data governance model, and your preferred method for connecting.

Most methods require elevated privileges on both the data source and the Databricks workspace to configure the necessary permissions to integrate systems. Users without these permissions should request help. See [Request access to data sources](broken-reference).

### Configure connections to external data systems

Databricks recommends several options for configuring connections to external data systems depending on your needs. The following table provides a high-level overview of these options:

| Option               | Description                                                                                                                                                                                                                                                                          |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Lakehouse Federation | Provides read-only access to data in enterprise data systems. Connections are configured through Unity Catalog at the catalog or schema level, syncing multiple tables with a single configuration. See What is Lakehouse Federation.                                                |
| Partner Connect      | Leverages technology partner solutions to connect to external data sources and automate ingesting data to the lakehouse. Some solutions also include reverse ETL and direct access to lakehouse data from external systems. See What is Databricks Partner Connect?                  |
| Drivers              | Databricks includes drivers for external data systems in each Databricks Runtime. You can optionally install third-party drivers to access data in other systems. You must configure connections for each table. Some drivers include write access. See Connect to external systems. |
| JDBC                 | Several included drivers for external systems build upon native JDBC support, and the JDBC option provides extensible options for configuring connections to other systems. You must configure connections for each table. See Query databases using JDBC.                           |

### Connect to streaming data sources

Databricks provides optimized connectors for many streaming data systems.

For all streaming data sources, you must generate credentials that provide access and load these credentials into Databricks. Databricks recommends storing credentials using secrets, because you can use secrets for all configuration options and in all access modes.

All data connectors for streaming sources support passing credentials using options when you define streaming queries. See Configure streaming data sources.

### Request access to data sources

In many organizations, most users do not have sufficient privileges on either Databricks or external data sources to configure data connections.

Your organization might have already configured access to a data source using one of the patterns described in the articles linked from this page. If your organization has a well-defined process for requesting access to data, Databricks recommends following that process.

If you’re uncertain how to gain access to a data source, this procedure might help you:

1. Use Catalog Explorer to view the tables and volumes that you can access. See What is Catalog Explorer?.
2. Ask your teammates or managers about the data sources that they can access.
   * Most organizations use groups synced from their identity provider (for example: Okta or Microsoft Entra ID (formerly Azure Active Directory)) to manage permissions for workspace users. If other members of your team can access data sources that you need access to, have a workspace admin add you to the correct group to grant you access.
   * If a particular table, volume, or data source was configured by a co-worker, that individual should have permissions to grant you access to the data.
3. Some organizations configure data access permissions through settings on compute clusters and SQL warehouses.
   * Access to data sources can vary by compute.
   * You can view the compute creator on the **Compute** tab. Reach out to the creator to ask about data sources that should be accessible.
