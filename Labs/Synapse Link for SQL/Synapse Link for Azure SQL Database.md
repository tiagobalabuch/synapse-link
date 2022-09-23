# Lab - Synapse Link for Azure SQL Database

> [!NOTE]
> Azure Synapse Link for SQL is currently in PREVIEW

In this lab, you will walk through a complete end-to-end scenario to analyze large operational datasets while minimizing the impact on the performance of mission-critical transactional workloads.

[Azure Synapse Link for Azure SQL](https://docs.microsoft.com/en-us/azure/synapse-analytics/synapse-link/sql-synapse-link-overview) is an automated system for replicating data from your transactional databases (currently in both SQL Server 2022 and Azure SQL Database) into a dedicated SQL pool in Azure Synapse Analytics.

You will configure the Azure environment to allow data to be transferred from an Azure SQL Database to an Azure Synapse Analytics Workspace using Azure Synapse Link for Azure SQL Database. You will also ingest data into Dedicate SQL Pool, explore spark pool together with Azure Machine Learning and Azure Cognitive Services on [SynapseML](https://microsoft.github.io/SynapseML/).

## Microsoft Learn & Technical Documentation

| Azure Services | Microsoft Learn | Technical documentation |
|:-------------- |:--------------- |:----------------------- |
| Azure SQL Database | [Explore Azure SQL Database](https://docs.microsoft.com/en-us/learn/paths/azure-sql-fundamentals/)| [Azure SQL Database Technical documentation](https://docs.microsoft.com/en-us/azure/azure-sql/?view=azuresql)|
|Azure Synapse Analytics | [Implement a Data Warehouse with Azure Synapse Analytics](https://docs.microsoft.com/en-us/learn/paths/realize-integrated-analytical-solutions-with-azure-synapse-analytics)| [Azure Synapse Analytics Technical Documentation](https://docs.microsoft.com/en-us/azure/synapse-analytics/)|
|Azure Data Lake Storage Gen2 | [Large Scale Data Processing with Azure Data Lake Storage Gen2](https://docs.microsoft.com/en-us/learn/paths/data-processing-with-azure-adls) | [Azure Data Lake Storage Gen2 Technical Documentation](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction)|
| SynapseML   |[SynapseML Overview](https://microsoft.github.io/SynapseML/)   |[SynapseML Technical Documentation](https://microsoft.github.io/SynapseML/docs/getting_started/installation/)|
|LightGBM |[LightGBM on Apache Spark](https://microsoft.github.io/SynapseML/docs/features/lightgbm/about/)| [LightGBM Quick Start Technical Documentation](https://lightgbm.readthedocs.io/en/latest/Quick-Start.html/)|

## Lab architecture

![azuresqldb-reference-architecture](../../media/Synapse%20Link%20for%20Azure%20SQL%20DB/azuresqldb-reference-architecture.png)

- Step 1 - [Configuring linked services for Azure ML](Synapse%20Link%20for%20Azure%20SQL%20Database.md#configuring-linked-services-for-azure-ml)
- Step 2 - [Configuring Synapse Link for Azure SQL Database](Synapse%20Link%20for%20Azure%20SQL%20Database.md#configuring-synapse-link-for-azure-sql-database)
- Step 3 - [Create an Azure SQL Database linked service](Synapse%20Link%20for%20Azure%20SQL%20Database.md#create-an-azure-sql-database-linked-service)
- Step 4 - [Create the Azure Synapse Link connection](Synapse%20Link%20for%20Azure%20SQL%20Database.md#create-the-azure-synapse-link-connection)
- Step 5 - [CExploring data with Notebooks using Synapse Spark Pool](Synapse%20Link%20for%20Azure%20SQL%20Database.md#exploring-data-with-notebooks-using-synapse-spark-pool)

## Configuring linked services for Azure ML

[Create an Azure ML linked service](https://docs.microsoft.com/en-us/azure/synapse-analytics/machine-learning/quickstart-integrate-azure-machine-learning#create-an-azure-ml-linked-service)

1. In Synapse Studio click on the 'Manage' icon in the left panel and navigate to 'Linked Services' menu option and click '*+New*'.
![synapse-studio-linked-service](../../media/Synapse%20Link%20for%20Cosmos%20DB/synapse-studio-linked-services.png)
2. Choose "Azure Machine Learning" click '*Continue*' to open up configuration settings.
![synapse-studio-linked-services-aml](../../media/Synapse%20Link%20for%20Cosmos%20DB/synapse-studio-linked-services-aml.png)
3. Name: **AzureMLServices**
4. Connect via IR: **AutoResoveIntegrationRuntime**
5. Authentication type: **System Assigned Managed Identity**
6. Account selection method: **From Azure Subscription**
   1. Azure Subscription: **Use your subscription**
   2. Azure Machine Learning workspace name: **aml-workspace-*suffix***
7. Click Test connection
8. Click '*Create*' to save the changes.

![synapse-studio-linked-service](../../media/Synapse%20Link%20for%20Cosmos%20DB/synapse-studio-linked-services-aml-settings.png)

## Configuring Synapse Link for Azure SQL Database

Before going straight to the point, there are a few steps you have to complete.
This [documentation](https://docs.microsoft.com/en-us/azure/synapse-analytics/synapse-link/connect-synapse-link-sql-database) explain all these steps but you can follow here as we will walk through them.

> [!IMPORTANT]
> Make sure you have deploy the Azure SQL Database before moving forward. Review the deploy instructions.

### Configure your source Azure SQL Database

1. Go to Azure portal, navigate to your Azure SQL Server
   1. Select Identity
   2. Then set System assigned managed identity to **On**
   ![azure-sql-db-identity-settings](../../media/Synapse%20Link%20for%20Azure%20SQL%20DB/azure-sql-db-identity-settings.png)

2. Navigate to **Azure Active Directory**
   1. Set admin: **use your identity** (e.g. tiagobalabuch@mycompany.com)
   2. [Provision Azure Ad Admin Azure SQL Database](https://docs.microsoft.com/en-us/azure/azure-sql/database/authentication-aad-configure?view=azuresql&tabs=azure-powershell#provision-azure-ad-admin-sql-database)
   ![azure-sql-db-ad-admin-settings](../../media/Synapse%20Link%20for%20Azure%20SQL%20DB/azure-sql-db-ad-admin-settings.png)

3. In the left panel, navigate to **SQL databases**
4. Click "**RetailDB**"
5. Navigate to Query editor (preview)
6. Connect using Active Directory authentication
   1. We are going to have our Synapse workspace connect to our Azure SQL Database using a managed identity. That is reason why we have set the Azure Active Directory admin on Azure SQL Server
7. Run the following script to provide the managed identity permission to the source database.

```sql
CREATE USER <workspace name> FROM EXTERNAL PROVIDER;
ALTER ROLE [db_owner] ADD MEMBER <workspace name>;
```

8. Replace <workspace name> for your workspace **synapse-link-*suffix***

```sql
CREATE USER [synapse-link-balabuch] FROM EXTERNAL PROVIDER;
ALTER ROLE [db_owner] ADD MEMBER [synapse-link-balabuch];
```

### Create your target Synapse SQL pool

During our deployment an Azure Synapse SQL Dedicated Pool was created. But if you haven't done it, follow this [steps](../Deploy/Readme.md#create-a-dedicated-sql-pool).

## Create an Azure SQL Database linked service

1. In Synapse Studio click on the 'Manage' icon in the left panel and navigate to 'Linked Services' menu option and click '*+New*'.
![synapse-studio-linked-service](../../media/Synapse%20Link%20for%20Cosmos%20DB/synapse-studio-linked-services.png)
2. Choose Azure SQL Database and click '*Continue*' to open up configuration settings.
 ![synapse-studio-link-choose-sql-db](../../media/Synapse%20Link%20for%20Azure%20SQL%20DB/synapse-studio-link-choose-sql-db.png)
3. Name: **AzureSQLDBLink**
4. Connect via IR: **AutoResoveIntegrationRuntime**
5. Account selection method: **From Azure Subscription**
   1. Azure Subscription: **Use your subscription**
   2. Server name: **sql-link-*suffix***
   3. Database Name: **RetailDB**
6. Authentication type: **System Assigned Managed Identity**
7. Click Test connection
8. Click '*Create*' to save the changes

![synapse-studio-create-link-sql-db-settings](../../media/Synapse%20Link%20for%20Azure%20SQL%20DB/synapse-studio-create-link-sql-db-settings.png)

> [!TIP]
> Pay attention on Managed identity name. This is the name you should have used when you were configuring your source Azure SQL Database.

## Create the Azure Synapse Link connection

It's time to really create our link between Azure SQL Database and Azure Synapse Analytics.

1. In Synapse Studio click on the 'Integrate' icon in the left panel and navigate to '*+ Link connection(Preview)*'.
![synapse-studio-create-link-sql-db](../../media/Synapse%20Link%20for%20Azure%20SQL%20DB/synapse-studio-create-link-sql-db.png)

2. Source type: **Azure SQL database**
3. Source linked service: **AzureSQLDBLink**
4. Source tables: **select all**

![synapse-studio-create-link-sql-db-settings-source](../../media/Synapse%20Link%20for%20Azure%20SQL%20DB/synapse-studio-create-link-sql-db-settings-source.png)

5. Click '*Continue*'
6. Target pool: **DW**

![synapse-studio-create-link-sql-db-settings-target.](../../media/Synapse%20Link%20for%20Azure%20SQL%20DB/synapse-studio-create-link-sql-db-settings-target.png)

7. Click '*Continue*'
8. Link connection name: **link-to-azure-sql-db**
9. Core count: **2 (+ 2 Driver cores)**
10. Mode: **Continuous**

![synapse-studio-create-link-sql-db-settings-connection](../../media/Synapse%20Link%20for%20Azure%20SQL%20DB/synapse-studio-create-link-sql-db-settings-connection.png)

11. Click '*OK*'
12. Review all the information
13. Change th structure type of your tables to **HEAP**
14. Select Publish all to save the new link connection to the service.

![synapse-studio-create-link-sql-db-final](../../media/Synapse%20Link%20for%20Azure%20SQL%20DB/synapse-studio-create-link-sql-db-final.png)

15. Then click *Start*
16. Wait a few minutes for the data to be replicated.
17. Once you linked started you can click 'Develop' icon in the left panel and click '+ SQL Script'
18. Run the following statement to get data from a table.

```sql
SELECT [id]
,[productCode]
,[basePrice]
,[wholeSaleCost]
 FROM [dbo].[Products]
```

## Exploring data with Notebooks using Synapse Spark Pool

Notebooks contain both computer code (e.g. python) and rich text elements (paragraph, equations, figures, links, etc…). Notebooks are both human-readable documents containing the analysis description and the results (figures, tables, etc..) as well as executable documents which can be run to perform data analysis.
They are an essential tool for data, prototyping, and learning!

There is one notebooks to you explore and learn:

- **Sales forecasting with SynapseML**

Create a new folder named "Azure SQL DB" and import this notebook.

[Create, develop, and maintain Synapse notebooks in Azure Synapse Analytics](https://docs.microsoft.com/en-us/azure/synapse-analytics/spark/apache-spark-development-using-notebooks)

First let's [import](https://docs.microsoft.com/en-us/azure/synapse-analytics/spark/apache-spark-development-using-notebooks#create-a-notebook)  these notebooks into the Azure Synapse Analytics workspace.

> [!TIP]
> Do not forget to Publish All

In Synapse Studio click on the 'Develop' icon in the left panel and navigate to the 'Notebooks' section.

In these notebooks we are going to leverage the power of no ELT/ETL with Azure Synapse Link for Azure SQL DB using PySpark.

### Sales forecasting with SynapseML

This notebook will cover:

- Reading data from  Dedicated SQL Pool using Connector for Apache Spark
  - We are going to read tables that we've just replicated from the Azure SQL DB without performing an ETL/ELT.
- Join & aggregate operational data across Dedicated SQL pool tables
- Perform Sales forecasting using SynapseML

_**This notebook uses Spark 3.2**_ - Attach to **demo** Apache Spark Pool

## Page Navigator

[Index: Table of Contents](../../Index.md)

[Azure Synapse Link Workshop](../../README.md)

[Prev: Lab 1 - Azure Synapse Link for Cosmos DB](../Synapse%20Link%20for%20Cosmos%20DB/Synapse%20Link%20for%20Cosmos%20DB.md)
