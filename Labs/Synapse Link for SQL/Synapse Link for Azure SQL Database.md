# Lab - Synapse Link for Azure SQL Database

> [!NOTE]
> Azure Synapse Link for SQL is currently in PREVIEW

In this lab, you will walk through a complete end-to-end scenario to analyze large operational datasets while minimizing the impact on the performance of mission-critical transactional workloads,

[Azure Synapse Link for Azure SQL](https://docs.microsoft.com/en-us/azure/synapse-analytics/synapse-link/sql-synapse-link-overview) is an automated system for replicating data from your transactional databases (currently in both SQL Server 2022 and Azure SQL Database) into a dedicated SQL pool in Azure Synapse Analytics.

You will configure the Azure environment to allow data to be transferred from an Azure SQL Database to an Azure Synapse Analytics Workspace using Azure Synapse Link for Azure SQL Database. You will also ingest data into Dedicate SQL Pool, explore spark pool together with Azure Machine Learning and Azure Cognitive Services on [SynapseML](https://microsoft.github.io/SynapseML/).

## Microsoft Learn & Technical Documentation

| Azure Services | Microsoft Learn | Technical documentation |
|:-------------- |:--------------- |:----------------------- |
| Azure SQL Database | [Explore Azure SQL Database](https://docs.microsoft.com/en-us/learn/paths/azure-sql-fundamentals/)| [Azure SQL Database Technical documentation](https://docs.microsoft.com/en-us/azure/azure-sql/?view=azuresql)|
|Azure Synapse Analytics | [Implement a Data Warehouse with Azure Synapse Analytics](https://docs.microsoft.com/en-us/learn/paths/realize-integrated-analytical-solutions-with-azure-synapse-analytics)| [Azure Synapse Analytics Technical Documentation](https://docs.microsoft.com/en-us/azure/synapse-analytics/)|
|Azure Data Lake Storage Gen2 | [Large Scale Data Processing with Azure Data Lake Storage Gen2](https://docs.microsoft.com/en-us/learn/paths/data-processing-with-azure-adls) | [Azure Data Lake Storage Gen2 Technical Documentation](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction)|
|Azure Cognitive Anomaly Detector Services| [Introduction to Anomaly Detector](https://docs.microsoft.com/en-us/learn/modules/intro-to-anomaly-detector/)| [Azure Cognitive Anomaly Detector Technical Documentation](https://docs.microsoft.com/en-us/azure/cognitive-services/anomaly-detector/)|
|Azure Machine Learning |[Build and operate machine learning solutions with Azure Machine Learning](https://docs.microsoft.com/en-us/learn/paths/build-ai-solutions-with-azure-ml-service/)| [Azure Machine Learning Technical Documentation](https://docs.microsoft.com/en-us/azure/machine-learning/)|
|Azure Key Vault    |[Configure and manage secrets in Azure Key Vault](https://docs.microsoft.com/bs-latn-ba/learn/modules/configure-and-manage-azure-key-vault/)   |[Azure Key Vault Technical Documentation](https://docs.microsoft.com/en-us/azure/key-vault/)|

# Lab architecture

![cosmosdb-reference-architecture](../../media/Synapse%20Link%20for%20Azure%20SQL%20DB/azuresqldb-reference-architecture.png)

- Step 1 - [Configuring linked services for Azure ML](Synapse%20Link%20for%20Cosmos%20DB.md#Configuring-linked-services-for-Azure-ML)
- Step 2 - [Configuring Synapse Link for CosmosDB](Synapse%20Link%20for%20Cosmos%20DB.md#Configuring-Synapse-Link-for-CosmosDB)
- Step 3 - [Validating Synapse Link for Cosmos DB](Synapse%20Link%20for%20Cosmos%20DB.md#Validating-Synapse-Link-for-Cosmos-DB)
- Step 4 - [Configuring linked services for Azure Key Vault](Synapse%20Link%20for%20Cosmos%20DB.md#Configuring-linked-services-for-Azure-Key-Vault)
- Step 5 - [Configuring linked services for Azure Cognitive Service](Synapse%20Link%20for%20Cosmos%20DB.md#Configuring-linked-services-for-Azure-Cognitive-Service)
- Step 6 - [Creating an Azure Cognitive Service linked service](Synapse%20Link%20for%20Cosmos%20DB.md#Creating-an-Azure-Cognitive-Service-linked-service)
- Step 7 -  [Exploring data with Notebooks using Synapse Spark Pool](Synapse%20Link%20for%20Cosmos%20DB.md#Exploring-data-with-Notebooks-using-Synapse-Spark-Pool)