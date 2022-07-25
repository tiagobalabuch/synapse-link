# Lab - Synapse Link for Cosmos DB

In this lab, you will walk through a complete end-to-end scenario to analyze large operational datasets while minimizing the impact on the performance of mission-critical transactional workloads,
Using [Azure Cosmos DB analytical store](https://docs.microsoft.com/en-us/azure/cosmos-db/analytical-store-introduction), a fully isolated column store, Azure Synapse Link enables no Extract-Transform-Load (ETL) analytics in Azure Synapse Analytics against your operational data at scale.

You will configure the Azure environment to allow data to be transferred from an Azure Cosmos DB to an Azure Synapse Analytics Workspace using Azure Synapse Link for Cosmos DB. You will also ingest data into Cosmos DB, explore spark pool together with Azure Machine Learning and Azure Cognitive Services on Synapse Spark (MMLSpark).

## Azure Services used in this lab

There will be a few resources to support an Azure Synapse link for CosmosDB:

- An Azure Resource Group
- An Azure Synapse Workspace
- An Azure Synapse SQL Pool **rever**
- An Azure Synapse Spark Pool
- An Azure Data Lake Storage Gen2 account
- A key vault to store the secrets
- Cosmos DB account - Core (SQL)
- CosmosDB Database (CosmosDemoDB)
- CosmosDB Containers with Analytical Store Enabled
  - Products
  - StoreDemoGraphics
  - RetailSales
  - IoTDeviceInfo
  - IoTSignals
- AML workspace
- Azure Cognitive Service
- PySpark Notebook to:
  - Ingest batch data into CosmosDB containers
  - Fetch data from CosmosDB
  - Join dataset together
  - Perform Sales Forecasting using Azure Synapse Link and Azure Machine Learning
- PySpark Notebook to:
  - Ingest stream and batch data into CosmosDB containers
  - Fetch data from CosmosDB
  - Join dataset together
  - Perform Anomaly Detection using Azure Synapse Link and Azure Cognitive Services on Synapse Spark Pool (MMLSpark)

## Microsoft Learn & Technical Documentation

| Azure Services | Microsoft Learn | Technical documentation |
|:-------------- |:--------------- |:----------------------- |
| Azure Cosmos DB | [Explore Azure Cosmos db](https://docs.microsoft.com/en-us/learn/modules/explore-azure-cosmos-db/)| [Cosmos DB Technical documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/)|
|Azure Synapse Analytics | [Implement a Data Warehouse with Azure Synapse Analytics](https://docs.microsoft.com/en-us/learn/paths/realize-integrated-analytical-solutions-with-azure-synapse-analytics)| [Azure Synapse Analytics Technical Documentation](https://docs.microsoft.com/en-us/azure/synapse-analytics/)|
|Azure Data Lake Storage Gen2 | [Large Scale Data Processing with Azure Data Lake Storage Gen2](https://docs.microsoft.com/en-us/learn/paths/data-processing-with-azure-adls) | [Azure Data Lake Storage Gen2 Technical Documentation](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction)|
|Azure Cognitive Anomaly Detector Services| [Introduction to Anomaly Detector](https://docs.microsoft.com/en-us/learn/modules/intro-to-anomaly-detector/)| [Azure Cognitive Anomaly Detector Technical Documentation](https://docs.microsoft.com/en-us/azure/cognitive-services/anomaly-detector/)|

# Lab architecture

![](../../media/cosmosdb-reference-architecture.png)

**IMPORTANT:** Some of the Azure services provisioned require globally unique name and a “-suffix” has been added to their names to ensure this uniqueness. Please take note of the suffix generated as you will need it for the following resources in this lab:

| Name | Type |
|:--------------- |:----------------------- |
|synapse-link-*suffix*| Azure Synapse Analytics Workspace|
|synapsedatalake*suffix*|Storage Account Data Lake Gen 2|
|anomaly-detector-*suffix*| Azure Synapse Analytics Workspace|
|cosmosdb-link-*suffix*| Azure Cosmos DB account|

## Deploying resources

Make sure you deploy all the resources in the following order:

| Azure Service | How to |
|:---- |:----- |
| Resource Group |[Create a Resource Group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups)
| Storage Account |[Create an Azure Data Lake Storage Gen2 account](https://docs.microsoft.com/en-us/azure/storage/blobs/create-data-lake-storage-account)|
|Anomaly Detector|[Create an Azure Cognitive Service Anomaly Detector](https://docs.microsoft.com/en-us/azure/cognitive-services/anomaly-detector/how-to/deploy-anomaly-detection-on-iot-edge#create-an-anomaly-detector-resource)|
|Azure Synapse Analytics | [Create an Azure Synapse Analytics](https://docs.microsoft.com/en-us/azure/synapse-analytics/quickstart-create-workspace)|
| Azure Cosmos DB|[Create an Azure Cosmos DB account](https://docs.microsoft.com/en-us/azure/cosmos-db/sql/create-cosmosdb-resources-portal#create-an-azure-cosmos-db-account)|
|Azure Key Vault| [Create an Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/general/quick-create-portal)|

After deploying all Azure services, it's time to set up some other resources like enable Synapse Link for Cosmos DB, Cosmos DB Database and containers and Spark Pool.

### Enable Synapse Link for Cosmos DB

Synapse Link creates a tight seamless integration between Azure Cosmos DB and Azure Synapse Analytics.

[Enable Synapse Link for Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/configure-synapse-link#enable-synapse-link)

### Create a Azure Cosmos DB Database and container

1. Start creating a new database into your Cosmos DB account. [How to create a database and container in Azure Cosmos DB SQL API](https://docs.microsoft.com/en-us/azure/cosmos-db/configure-synapse-link#create-analytical-ttl)

   - **Database id**: CosmosDBDemo
   - **Share throughput across containers:** Enable
   - **Database throughput(autoscale)**: Autoscale
   - **Database Max RU/s**: 4000

2. Now, you have to create five containers:

   - *IoTDeviceInfo*
     - Partition Key: /id
     - ***Analytical store***: ON
   - *IoTSignals*
     - Partition Key: /id
     - ***Analytical store***: ON
   - *Product*
     - Partition Key: /id
     - ***Analytical store***: ON
   - *RetailSales*
     - Partition Key: /id
     - ***Analytical store***: ON
   - *StoreDemoGraphics*
     - Partition Key: /id
     - ***Analytical store***: ON

> [!WARNING]
>
> Keep in mind that in all container you must enable analytical store

![](../../media/cosmosdb-database-containers.png)

### Create a Spark pool

Apache Spark is a parallel processing framework that supports in-memory processing to boost the performance of big data analytic applications and
An Apache Spark pool provides open-source big data compute capabilities.

1. Start creating a Apache Spark pool into you Azure Synapse Analytics workspace. [Create a new serverless Apache Spark pool](https://docs.microsoft.com/en-us/azure/synapse-analytics/quickstart-create-apache-spark-pool-portal).
   
- **Apache Spark pool name**: demo
- **Node size family**: Memory Optimized
- **Node size**: Medium (8 vCores / 64 GB)
- **Autoscale**: Enable
- **Automatic pausing**: Enable
- **Apache Spark**: 3.2
