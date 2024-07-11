# What is Databricks?

Databricks is a unified, open analytics platform for building, deploying, sharing, and maintaining enterprise-grade data, analytics, and AI solutions at scale. The Databricks Data Intelligence Platform integrates with cloud storage and security in your cloud account, and manages and deploys cloud infrastructure on your behalf.

### How does a data intelligence platform work?

Databricks uses generative AI with the data lakehouse to understand the unique semantics of your data. Then, it automatically optimizes performance and manages infrastructure to match your business needs.

Natural language processing learns your business’s language, so you can search and discover data by asking a question in your own words. Natural language assistance helps you write code, troubleshoot errors, and find answers in documentation.

Finally, your data and AI applications can rely on strong governance and security. You can integrate APIs such as OpenAI without compromising data privacy and IP control.

### What is Databricks used for?

Databricks provides tools that help you connect your sources of data to one platform to process, store, share, analyze, model, and monetize datasets with solutions from BI to generative AI.

The Databricks workspace provides a unified interface and tools for most data tasks, including:

* Data processing scheduling and management, in particular ETL
* Generating dashboards and visualizations
* Managing security, governance, high availability, and disaster recovery
* Data discovery, annotation, and exploration
* Machine learning (ML) modeling, tracking, and model serving
* Generative AI solutions

### Managed integration with open source

Databricks has a strong commitment to the open source community. Databricks manages updates of open source integrations in the Databricks Runtime releases. The following technologies are open source projects originally created by Databricks employees:

* [Delta Lake](https://delta.io/) and [Delta Sharing](https://delta.io/sharing)
* [MLflow](https://mlflow.org/)
* [Apache Spark](https://spark.apache.org/) and [Structured Streaming](https://spark.apache.org/streaming/)
* [Redash](https://redash.io/)

### How does Databricks work with AWS?

The Databricks platform architecture comprises two primary parts:

* The infrastructure used by Databricks to deploy, configure, and manage the platform and services.
* The customer-owned infrastructure managed in collaboration by Databricks and your company.

Unlike many enterprise data companies, Databricks does not force you to migrate your data into proprietary storage systems to use the platform. Instead, you configure a Databricks workspace by configuring secure integrations between the Databricks platform and your cloud account, and then Databricks deploys compute clusters using cloud resources in your account to process and store data in object storage and other integrated services you control.

Unity Catalog further extends this relationship, allowing you to manage permissions for accessing data using familiar SQL syntax from within Databricks.

Databricks workspaces meet the security and networking requirements of [some of the world’s largest and most security-minded companies](https://www.databricks.com/customers). Databricks makes it easy for new users to get started on the platform. It removes many of the burdens and concerns of working with cloud infrastructure, without limiting the customizations and control experienced data, operations, and security teams require.

### What are common use cases for Databricks?

Use cases on Databricks are as varied as the data processed on the platform and the many personas of employees that work with data as a core part of their job. The following use cases highlight how users throughout your organization can leverage Databricks to accomplish tasks essential to processing, storing, and analyzing the data that drives critical business functions and decisions.

### Build an enterprise data lakehouse

The data lakehouse combines the strengths of enterprise data warehouses and data lakes to accelerate, simplify, and unify enterprise data solutions. Data engineers, data scientists, analysts, and production systems can all use the data lakehouse as their single source of truth, allowing timely access to consistent data and reducing the complexities of building, maintaining, and syncing many distributed data systems. See What is a data lakehouse?.

### ETL and data engineering

Whether you’re generating dashboards or powering artificial intelligence applications, data engineering provides the backbone for data-centric companies by making sure data is available, clean, and stored in data models that allow for efficient discovery and use. Databricks combines the power of Apache Spark with Delta Lake and custom tools to provide an unrivaled ETL (extract, transform, load) experience. You can use SQL, Python, and Scala to compose ETL logic and then orchestrate scheduled job deployment with just a few clicks.

Delta Live Tables simplifies ETL even further by intelligently managing dependencies between datasets and automatically deploying and scaling production infrastructure to ensure timely and accurate delivery of data per your specifications.

Databricks provides a number of custom tools for [data ingestion](broken-reference), including Auto Loader, an efficient and scalable tool for incrementally and idempotently loading data from cloud object storage and data lakes into the data lakehouse.

### Machine learning, AI, and data science

Databricks machine learning expands the core functionality of the platform with a suite of tools tailored to the needs of data scientists and ML engineers, including MLflow and [Databricks Runtime for Machine Learning](broken-reference).

#### Large language models and generative AI

Databricks Runtime for Machine Learning includes libraries like Hugging Face Transformers that allow you to integrate existing pre-trained models or other open-source libraries into your workflow. The Databricks MLflow integration makes it easy to use the MLflow tracking service with transformer pipelines, models, and processing components. In addition, you can integrate [OpenAI](https://platform.openai.com/docs/introduction) models or solutions from partners like [John Snow Labs](about:blank/machine-learning/reference-solutions/natural-language-processing.html#john-snow-labs) in your Databricks workflows.

With Databricks, you can customize a LLM on your data for your specific task. With the support of open source tooling, such as Hugging Face and DeepSpeed, you can efficiently take a foundation LLM and start training with your own data to have more accuracy for your domain and workload.

In addition, Databricks provides AI functions that SQL data analysts can use to access LLM models, including from OpenAI, directly within their data pipelines and workflows. See AI Functions on Databricks.

### Data warehousing, analytics, and BI

Databricks combines user-friendly UIs with cost-effective compute resources and infinitely scalable, affordable storage to provide a powerful platform for running analytic queries. Administrators configure scalable compute clusters as SQL warehouses, allowing end users to execute queries without worrying about any of the complexities of working in the cloud. SQL users can run queries against data in the lakehouse using the SQL query editor or in notebooks. Notebooks support Python, R, and Scala in addition to SQL, and allow users to embed the same visualizations available in legacy dashboards alongside links, images, and commentary written in markdown.

### Data governance and secure data sharing

Unity Catalog provides a unified data governance model for the data lakehouse. Cloud administrators configure and integrate coarse access control permissions for Unity Catalog, and then Databricks administrators can manage permissions for teams and individuals. Privileges are managed with access control lists (ACLs) through either user-friendly UIs or SQL syntax, making it easier for database administrators to secure access to data without needing to scale on cloud-native identity access management (IAM) and networking.

Unity Catalog makes running secure analytics in the cloud simple, and provides a division of responsibility that helps limit the reskilling or upskilling necessary for both administrators and end users of the platform. See What is Unity Catalog?.

The lakehouse makes data sharing within your organization as simple as granting query access to a table or view. For sharing outside of your secure environment, Unity Catalog features a managed version of [Delta Sharing](broken-reference).

### DevOps, CI/CD, and task orchestration

The development lifecycles for ETL pipelines, ML models, and analytics dashboards each present their own unique challenges. Databricks allows all of your users to leverage a single data source, which reduces duplicate efforts and out-of-sync reporting. By additionally providing a suite of common tools for versioning, automating, scheduling, deploying code and production resources, you can simplify your overhead for monitoring, orchestration, and operations. Workflows schedule Databricks notebooks, SQL queries, and other arbitrary code. Git folders let you sync Databricks projects with a number of popular git providers. For a complete overview of tools, see Developer tools and guidance.

### Real-time and streaming analytics

Databricks leverages Apache Spark Structured Streaming to work with streaming data and incremental data changes. Structured Streaming integrates tightly with Delta Lake, and these technologies provide the foundations for both Delta Live Tables and Auto Loader. See Streaming on Databricks.
