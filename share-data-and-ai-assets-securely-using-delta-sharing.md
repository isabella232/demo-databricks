# Share data and AI assets securely using Delta Sharing

* Documentation
* Share data and AI assets securely using Delta Sharing
*

This article introduces Delta Sharing in Databricks, the secure data sharing platform that lets you share data and AI assets in Databricks with users outside your organization, whether those users use Databricks or not.

Important

The Delta Sharing articles on this site focus on sharing Databricks data, notebooks, and AI models. Delta Sharing is also available as an [open-source project](https://delta.io/sharing) that you can use to share Delta tables from other platforms. Delta Sharing also provides the backbone for [Databricks Marketplace](broken-reference), an open forum for exchanging data products.

### What is Delta Sharing?

[Delta Sharing](https://delta.io/sharing) is an [open protocol](https://github.com/delta-io/delta-sharing/blob/main/PROTOCOL.md) developed by Databricks for secure data sharing with other organizations regardless of the computing platforms they use.

There are three ways to share data using Delta Sharing:

1.  **The Databricks-to-Databricks sharing protocol**, which lets you share data and AI assets from your Unity Catalog-enabled workspace with users who also have access to a Unity Catalog-enabled Databricks workspace.

    This approach uses the Delta Sharing server that is built into Databricks. It supports some Delta Sharing features that are not suppported in the other protocols, including notebook sharing, Unity Catalog volume sharing, Unity Catalog AI model sharing, Unity Catalog data governance, auditing, and usage tracking for both providers and recipients. The integration with Unity Catalog simplifies setup and governance for both providers and recipients and improves performance.

    See Share data using the Delta Sharing Databricks-to-Databricks protocol (for providers).
2.  **The Databricks open sharing protocol**, which lets you share tabular data that you manage in a Unity Catalog-enabled Databricks workspace with users on any computing platform.

    This approach uses the Delta Sharing server that is built into Databricks and is useful when you manage data using Unity Catalog and want to share it with users who don’t use Databricks or don’t have access to a Unity Catalog-enabled Databricks workspace. The integration with Unity Catalog on the provider side simplifies setup and governance for providers.

    See Share data using the Delta Sharing open sharing protocol (for providers).
3.  **A customer-managed implementation of the open-source Delta Sharing server**, which lets you share from any platform to any platform, whether Databricks or not.

    The Databricks documentation does not cover instructions for setting up your own Delta Sharing server. See [github.com/delta-io/delta-sharing](https://github.com/delta-io/delta-sharing).

### Shares, providers, and recipients

The primary concepts underlying Delta Sharing in Databricks are _shares_, _providers_, and _recipients_.

#### What is a share?

In Delta Sharing, a _share_ is a read-only collection of tables and table partitions that a provider wants to share with one or more recipients. If your recipient uses a Unity Catalog-enabled Databricks workspace, you can also include notebook files, views (including dynamic views that restrict access at the row and column level), Unity Catalog volumes, and Unity Catalog models in a share.

You can add or remove tables, views, volumes, models, and notebook files from a share at any time, and you can assign or revoke data recipient access to a share at any time.

In a Unity Catalog-enabled Databricks workspace, a share is a securable object registered in Unity Catalog. If you remove a share from your Unity Catalog metastore, all recipients of that share lose the ability to access it.

See Create and manage shares for Delta Sharing.

#### What is a provider?

A _provider_ is an entity that shares data with a recipient. If you are a provider and you want to take advantage of the built-in Databricks Delta Sharing server and manage shares and recipients using Unity Catalog, you need at least one Databricks workspace that is enabled for Unity Catalog. You do not need to migrate all of your existing workspaces to Unity Catalog. You can simply create a new Unity Catalog-enabled workspace for your Delta Sharing needs.

If a recipient is on a Unity Catalog-enabled Databricks workspace, the provider is also a Unity Catalog securable object that represents the provider organization and associates that organization with a set of shares.

#### What is a recipient?

A _recipient_ is an entity that receives shares from a provider. In Unity Catalog, a share is a securable object that represents an organization and associates it with a credential or secure sharing identifier that allows that organization to access one or more shares.

As a data provider (sharer), you can define multiple recipients for any given Unity Catalog metastore, but if you want to share data from multiple metastores with a particular user or group of users, you must define the recipient separately for each metastore. A recipient can have access to multiple shares.

If a provider deletes a recipient from their Unity Catalog metastore, that recipient loses access to all shares it could previously access.

See Create and manage data recipients for Delta Sharing.

### Open sharing versus Databricks-to-Databricks sharing

This section describes the two protocols for sharing from a Databricks workspace that is enabled for Unity Catalog.

Note

This section assumes that the provider is on a Unity Catalog-enabled Databricks workspace. To learn about setting up an open-source Delta Sharing server to share from a non-Databricks platform or non-Unity Catalog workspace, see [github.com/delta-io/delta-sharing](https://github.com/delta-io/delta-sharing).

The way a provider uses Delta Sharing in Databricks depends on who they are sharing data with:

* _Open sharing_ lets you share data with any user, whether or not they have access to Databricks.
* _Databricks-to-Databricks sharing_ lets you share data with Databricks users whose workspace is attached to a Unity Catalog metastore that is different from yours. Databricks-to-Databricks also supports notebook, volume, and model sharing, which is not available in open sharing.

#### What is open Delta Sharing?

If you want to share data with users outside of your Databricks workspace, regardless of whether they use Databricks, you can use open Delta Sharing to share your data securely. As a data provider, you generate a token and share it securely with the recipient. They use the token to authenticate and get read access to the tables you’ve included in the shares you’ve given them access to.

Recipients can access the shared data using many computing tools and platforms, including:

* Databricks
* Apache Spark
* Pandas
* Power BI

For a full list of Delta Sharing connectors and information about how to use them, see the [Delta Sharing](https://delta.io/sharing) documentation.

See also Share data using the Delta Sharing open sharing protocol (for providers).

#### What is Databricks-to-Databricks Delta Sharing?

If you want to share data with users who have a Databricks workspace that is enabled for Unity Catalog, you can use Databricks-to-Databricks Delta Sharing. Databricks-to-Databricks sharing lets you share data with users in other Databricks accounts, whether they’re on AWS, Azure, or GCP. It’s also a great way to securely share data across different Unity Catalog metastores in your own Databricks account. Note that there is no need to use Delta Sharing to share data between workspaces attached to the same Unity Catalog metastore, because in that scenario you can use Unity Catalog itself to manage access to data across workspaces.

One advantage of Databricks-to-Databricks sharing is that the share recipient doesn’t need a token to access the share, and the provider doesn’t need to manage recipient tokens. The security of the sharing connection—including all identity verification, authentication, and auditing—is managed entirely through Delta Sharing and the Databricks platform. Another advantage is the ability to share Databricks notebook files, views, Unity Catalog volumes, and Unity Catalog models.

See also Share data using the Delta Sharing Databricks-to-Databricks protocol (for providers).

### How do provider admins set up Delta Sharing?

This section gives an overview of how providers can enable Delta Sharing and initiate sharing from a Unity Catalog-enabled Databricks workspace. For open-source Delta Sharing, see [github.com/delta-io/delta-sharing](https://github.com/delta-io/delta-sharing).

Databricks-to-Databricks sharing between Unity Catalog metastores in the same account is always enabled. If you are a provider who wants to enable Delta Sharing to share data with Databricks workspaces in other accounts or non-Databricks clients, a Databricks account admin or metastore admin performs the following setup steps (at a high level):

1.  Enable Delta Sharing for the Unity Catalog metastore that manages the data you want to share.

    Note

    You do not need to enable Delta Sharing on your metastore if you intend to use Delta Sharing to share data only with users on other Unity Catalog metastores in your account. Metastore-to-metastore sharing within a single Databricks account is enabled by default.

    See [Enable Delta Sharing on a metastore](about:blank/set-up.html#enable).
2.  Create a share that includes data assets registered in the Unity Catalog metastore.

    If you are sharing with a non-Databricks recipient (known as open sharing) you can include tables in the Delta or Parquet format. If you plan to use [Databricks-to-Databricks sharing](broken-reference), you can also add views, Unity Catalog volumes, Unity Catalog models, and notebook files to a share.

    See Create and manage shares for Delta Sharing.
3.  Create a recipient.

    See Create and manage data recipients for Delta Sharing.

    If your recipient is not a Databricks user, or does not have access to a Databricks workspace that is enabled for Unity Catalog, you must use [open sharing](broken-reference). A set of token-based credentials is generated for that recipient.

    If your recipient has access to a Databricks workspace that is enabled for Unity Catalog, you can use [Databricks-to-Databricks sharing](broken-reference), and no token-based credentials are required. You request a _sharing identifier_ from the recipient and use it to establish the secure connection.

    Tip

    Use yourself as a test recipient to try out the setup process.
4.  Grant the recipient access to one or more shares.

    See Manage access to Delta Sharing data shares (for providers).
5.  Send the recipient the information they need to connect to the share (open sharing only).

    See [Send the recipient their connection information](about:blank/create-recipient.html#send).

    For open sharing, use a secure channel to send the recipient an activation link that allows them to download their token-based credentials.

    For Databricks-to-Databricks sharing, the data included in the share becomes available in the recipient’s Databricks workspace as soon as you grant them access to the share.

The recipient can now access the shared data.

### How do recipients access the shared data?

Recipients access shared data assets in read-only format. Shared notebook files are read-only, but they can be cloned and then modified and run in the recipient workspace just like any other notebook.

Secure access depends on the sharing model:

* Open sharing (recipient does not have a Databricks workspace enabled for Unity Catalog): The recipient provides the credential whenever they access the data in their tool of choice, including Apache Spark, pandas, Power BI, Databricks, and many more. See Read data shared using Delta Sharing open sharing (for recipients).
* Databricks-to-Databricks (recipient workspace is enabled for Unity Catalog): The recipient accesses the data using Databricks. They can use Unity Catalog to grant and deny access to other users in their Databricks account. See Read data shared using Databricks-to-Databricks Delta Sharing (for recipients).

Whenever the data provider updates data tables or volumes in their own Databricks account, the updates appear in near real time in the recipient’s system.

### How do you keep track of who is sharing and accessing shared data?

Data providers on Unity Catalog-enabled Databricks workspaces can use Databricks audit logging and system tables to monitor the creation and modification of shares and recipients, and can monitor recipient activity on shares. See Audit and monitor data sharing.

Data recipients who use shared data in a Databricks workspace can use Databricks audit logging and system tables to understand who is accessing which data. See Audit and monitor data sharing.

### Restricting access at the row and column level

You can share dynamic views that restrict access to certain table data based on recipient properties. Dynamic view sharing requires the Databricks-to-Databricks sharing flow. See [Add dynamic views to a share to filter rows and columns](about:blank/create-share.html#dynamic-views).

### Delta Lake feature support matrix

Delta Sharing supports most Delta Lake features when you share a table. This support matrix lists Delta features that require specific versions of Databricks Runtime and the open-source Delta Sharing Spark connector, along with unsupported features.

| Feature           | Provider                                               | Databricks recipient                                                                                                        | Open source recipient    |
| ----------------- | ------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- | ------------------------ |
| Deletion vectors  | Sharing tables with this feature is in Public Preview. | <ul><li>Databricks Runtime 14.1+ for batch queries</li><li>Databricks Runtime 14.2+ for CDF and streaming queries</li></ul> | delta-sharing-spark 3.1+ |
| Column mapping    | Sharing tables with this feature is in Public Preview. | <ul><li>Databricks Runtime 14.1+ for batch queries</li><li>Databricks Runtime 14.2+ for CDF and streaming queries</li></ul> | delta-sharing-spark 3.1+ |
| Uniform format    | Sharing tables with this feature is in Public Preview. | <ul><li>Databricks Runtime 14.1+ for batch queries</li><li>Databricks Runtime 14.2+ for CDF and streaming queries</li></ul> | delta-sharing-spark 3.1+ |
| v2Checkpoint      | Not supported                                          | Not supported                                                                                                               | Not supported            |
| TimestampNTZ      | Not supported                                          | Not supported                                                                                                               | Not supported            |
| Liquid clustering | Not supported                                          | Not supported                                                                                                               | Not supported            |

### Delta Sharing FAQs

The following are frequently asked questions about Delta Sharing.

#### Do I need Unity Catalog to use Delta Sharing?

No, you do not need Unity Catalog to share (as a provider) or consume shared data (as a recipient). However, Unity Catalog provides benefits such as support for non-tabular and AI asset sharing, out-of-the-box governance, simplicity, and query performance.

Providers can share data in two ways:

*   Put the assets to share under Unity Catalog management and share them using the built-in Databricks Delta Sharing server.

    You do do not need to migrate all assets to Unity Catalog. You need only one Databricks workspace that is enabled for Unity Catalog to manage assets that you want to share. In some accounts, new workspaces are enabled for Unity Catalog automatically. See [Automatic enablement of Unity Catalog](about:blank/data-governance/unity-catalog/get-started.html#enablement).
* Implement the [open Delta Sharing server](https://github.com/delta-io/delta-sharing) to share data, without necessarily using your Databricks account.

Recipients can consume data in two ways:

* Without a Databricks workspace. Use open source Delta Sharing connectors that are available for many data platforms, including Power BI, pandas, and open source Apache Spark. See Read data shared using Delta Sharing open sharing (for recipients) and the [Delta Sharing open source project](https://delta.io/sharing/).
*   In a Databricks workspace. Recipient workspaces don’t need to be enabled for Unity Catalog, but there are advantages of governance, simplicity, and performance if they are.

    Recipient organizations who want these advantages don’t need to migrate all assets to Unity Catalog. You need only one Databricks workspace that is enabled for Unity Catalog to manage assets that are shared with you. In some accounts, new workspaces are enabled for Unity Catalog automatically. See [Automatic enablement of Unity Catalog](about:blank/data-governance/unity-catalog/get-started.html#enablement).

See Read data shared using Delta Sharing open sharing (for recipients) and Read data shared using Databricks-to-Databricks Delta Sharing (for recipients).

#### Do I need to be a Databricks customer to use Delta Sharing?

No, Delta Sharing is an open protocol. You can share non-Databricks data with recipients on any data platform. Providers can configure an open Delta Sharing server to share from any computing platform. Recipients can consume shared data using open source Delta Sharing connectors for many data products, including Power BI, pandas, and open source Spark.

However, using Delta Sharing on Databricks, especially sharing from a Unity Catalog-enabled workspace, has many advantages.

For details, see the first question in this FAQ.

#### Does Delta Sharing incur egress costs?

Delta Sharing within a region incurs no egress cost. Unlike other data sharing platforms, Delta Sharing does not require data replication. This model has many advantages, but it means that your cloud vendor may charge data egress fees when you share data across clouds or regions. Databricks supports sharing from Cloudflare R2 (Public Preview), which incurs no egress fees, and provides other tools and recommendations to monitor and avoid egress fees. See Monitor and manage Delta Sharing egress costs (for providers).

#### Isn’t it insecure to use pre-signed URLs?

Delta Sharing uses pre-signed URLs to provide temporary access to a file in object storage. They are only given to recipients that already have access to the shared data. They are secure because they are short-lived and don’t expand the level of access beyond what recipients have already been granted.

#### Are the tokens used in the Delta Sharing open sharing protocol secure?

Because Delta Sharing enables cross-platform sharing—unlike other available data sharing platforms—the sharing protocol requires an open token. Providers can ensure token security by configuring the token lifetime, setting networking controls, and revoking access on demand. In addition, the token does not expand the level of access beyond what recipients have already been granted. See [Security considerations for tokens](about:blank/create-recipient.html#security-considerations).

If you prefer not to use tokens to manage access to recipient shares, you should use Databricks-to-Databricks sharing or contact your Databricks account team for alternatives.

#### Does Delta Sharing support view sharing?

Yes, Delta Sharing supports view sharing. See [Add views to a share](about:blank/create-share.html#views).

To learn about planned enhancements to viewing sharing, contact your Databricks account team.

### Limitations

* Tabular data must be in the Delta table format. You can easily convert Parquet tables to Delta—and back again. See CONVERT TO DELTA.
* View sharing is supported only in Databricks-to-Databricks sharing. Shareable views must be defined on Delta tables or other shareable views. See [Add views to a share](about:blank/create-share.html#views) (for providers) and [Read shared views](about:blank/read-data-databricks.html#views) (for recipients).
* Notebook sharing is supported only in Databricks-to-Databricks sharing. See [Add notebook files to a share](about:blank/create-share.html#add-remove-notebook-files) and Read data shared using Databricks-to-Databricks Delta Sharing (for recipients).
* Volume sharing is supported only in Databricks-to-Databricks sharing. See [Add volumes to a share](about:blank/create-share.html#volumes) (for providers) and Read data shared using Databricks-to-Databricks Delta Sharing (for recipients).
* Model sharing is supported only in Databricks-to-Databricks sharing. See [Add models to a share](about:blank/create-share.html#models) (for providers) and Read data shared using Databricks-to-Databricks Delta Sharing (for recipients).
* There are limits on the number of files in metadata allowed for a shared table. To learn more, see [Resource limit exceeded errors](about:blank/troubleshooting.html#resource-limits).
* Schemas named `information_schema` cannot be imported into a Unity Catalog metastore, because that schema name is reserved in Unity Catalog.
* Table constraints (primary and foreign key constraints) are not available in shared tables.

### Resource quotas

The values below indicate the quotas for Delta Sharing resources. Quota values below are expressed relative to the parent object in Unity Catalog.

| Object     | Parent    | Value |
| ---------- | --------- | ----- |
| provider   | metastore | 1000  |
| recipients | metastore | 5000  |
| shares     | metastore | 1000  |
| tables     | share     | 1000  |
| volumes    | share     | 1000  |
| models     | share     | 1000  |
| schemas    | share     | 500   |
| notebooks  | share     | 100   |

If you expect to exceed these resource limits, contact your Databricks account team.

### Next steps

* Enable your Databricks account for Delta Sharing
* Create shares
* Create recipients
* Learn more about the open sharing and Databricks-to-Databricks sharing models
* Learn how recipients access shared data

***
