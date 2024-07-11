# Discover data

Databricks provides a suite of tools and products that simplify the discovery of data assets that are accessible through the Databricks Data Intelligence Platform. This article provides an opinionated overview of how you can discover and preview data that has already been configured for access in your workspace.

* To connect to data sources, see [Connect to data sources](broken-reference).
* For information about gaining access to data in the Databricks Marketplace, see [What is Databricks Marketplace?](broken-reference).

Topics in this section focus on exploring data objects and data files. If you’re looking for information about working with assets such as notebooks, SQL queries, libraries, and models, see Navigate the workspace.

If you’re seeking guidance around generating summary statistics for datasets or other tasks associated with exploratory data analysis (EDA), see Exploratory data analysis on Databricks: Tools and techniques.

### How can you discover data assets?

Data discovery tools on Databricks fall into the following general categories:

* AI-assisted insights, summary, and search.
* Keyword search.
* Catalog exploration using the UI.
* Programmatic listing and metadata exploration.

Data discovery tools are optimized for data governed by Unity Catalog. Data assets that have not been registered as Unity Catalog objects might not be discoverable using some of these approaches.

### Find data using the UI

Catalog Explorer provides tools for exploring and governing data assets. You access Catalog Explorer using the ![Catalog icon](<../.gitbook/assets/data icon.png>) **Catalog** in the workspace sidebar. See What is Catalog Explorer?.

Notebooks and the SQL query editor also provide a catalog navigator for exploring database objects. Click the **Catalog** icon in these interfaces to expand or collapse the catalog navigator without leaving from your code editor.

Once you’ve discovered a dataset of interest, you can use the **Insights** tab to learn how the data is being used in your workspace. See View frequent queries and users of a table.

### Search for tables in your lakehouse

You can use the search bar in Databricks to find tables registered to Unity Catalog. You can either perform a keyword search or use semantic search to find datasets or columns that relate to your search query. Search only returns results for tables that you have permission to see. Search reviews table names, column names, table comments, and column comments. See Search for workspace objects.
