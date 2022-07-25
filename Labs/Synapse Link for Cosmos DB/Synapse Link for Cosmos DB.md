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