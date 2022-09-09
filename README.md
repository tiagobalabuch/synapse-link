# Azure Synapse Link Workshop

In this workshop you will learn about the main concepts related to Azure Synapse Link and how it can be used to implement a modern data platform architecture. You will learn how Azure Synapse Link you can leverage to establish a solid data platform to quickly ingest, process and visualize data from a Azure Cosmos DB and Azure SQL Database data sources. The reference architecture you will build as part of this exercise will provide you a great level of details.

> [!IMPORTANT]
> Assume you have the right permission to create azure resources, and objects.

## Refereces

- [Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/synapse-link)
- [Dataverse](https://docs.microsoft.com/en-us/power-apps/maker/data-platform/export-to-data-lake)
- [SQL](https://docs.microsoft.com/azure/synapse-analytics/synapse-link/sql-synapse-link-overview)

## Azure Synapse Analytics

Azure Synapse is an enterprise analytics service that accelerates time to insight across data warehouses and big data systems. Azure Synapse brings together the best of **SQL** technologies used in enterprise data warehousing, **Spark** technologies used for big data, **Data Explorer** for log and time series analytics, Pipelines for data integration and ETL/ELT, and deep integration with other Azure services such as **Power BI**, **CosmosDB**, and **AzureML**.

![synapse-architecture](media/synapse-architecture.png)

Learn more about [Azure Synapse Analytics](https://azure.microsoft.com/en-au/services/synapse-analytics)

## [Lab 0 - Deploy](Labs/Deploy/Readme.md)

In this section you will provision all Azure resources required to complete labs. We will use a pre-defined ARM template with the definition of all Azure services used to ingest, store, process and visualize data.

## [Lab 1 - Azure Synapse Link for Cosmos DB](Labs/Synapse%20Link%20for%20Cosmos%20DB/Synapse%20Link%20for%20Cosmos%20DB.md)

In this lab, you will walk through a complete end-to-end scenario to analyze large operational datasets while minimizing the impact on the performance of mission-critical transactional workloads,
Using [Azure Cosmos DB analytical store](https://docs.microsoft.com/en-us/azure/cosmos-db/analytical-store-introduction), a fully isolated column store, Azure Synapse Link enables no Extract-Transform-Load (ETL) analytics in Azure Synapse Analytics against your operational data at scale.

You will configure the Azure environment to allow data to be transferred from an Azure Cosmos DB to an Azure Synapse Analytics Workspace using Azure Synapse Link for Cosmos DB. You will also ingest data into Cosmos DB, explore spark pool together with Azure Machine Learning and Azure Cognitive Services on [SynapseML](https://microsoft.github.io/SynapseML/).

![cosmosdb-reference-architecture](media/Synapse%20Link%20for%20Cosmos%20DB/cosmosdb-reference-architecture.png)

## [Lab 2 - Azure Synapse Link for Azure SQL Database](Labs/Synapse%20Link%20for%20SQL/Synapse%20Link%20for%20Azure%20SQL%20Database.md)

In this lab, you will walk through a complete end-to-end scenario to analyze large operational datasets while minimizing the impact on the performance of mission-critical transactional workloads.

[Azure Synapse Link for Azure SQL](https://docs.microsoft.com/en-us/azure/synapse-analytics/synapse-link/sql-synapse-link-overview) is an automated system for replicating data from your transactional databases (currently in both SQL Server 2022 and Azure SQL Database) into a dedicated SQL pool in Azure Synapse Analytics.

![azuresqldb-reference-architecture](media/Synapse%20Link%20for%20Azure%20SQL%20DB/azuresqldb-reference-architecture.png)
## Page Navigator

[Index: Table of Contents](Index.md)

[Next: Lab 0 - Deploy](Labs/Deploy/Readme.md)
