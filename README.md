# Azure Synapse Link Workshop

In this workshop you will learn about the main concepts related to Azure Synapse Link and how it can be used to implement a modern data platform architecture. You will learn how Azure Synapse Link you can leverage to establish a solid data platform to quickly ingest, process and visualize data from a Cosmos DB, Azure SQL Database data sources. The reference architecture you will build as part of this exercise will provide you a great level of details.

In the exercises in this lab you will build a end to end scenarios using sample data. The workshop was designed to progressively implement a modern data platform architecture.

**Keep in mind**

Assume you have the right permission to create azure resouces and synapse objects

# Rever esses bullets points

- The reference architecture proposed in this workshop aims to explain just enough of the role of each of the Azure Data Services included in the overall modern data platform architecture. This workshop does not replace the need of in-depth training on each Azure service covered.

- The services covered in this course are only a subset of a much larger family of Azure services. Similar outcomes can be achieved by leveraging other services and/or features not covered by this workshop. Specific business requirements may require the use of different services or features not included in this workshop.

- Some concepts presented in this course can be quite complex and you may need to seek more information from different sources to compliment your understanding of the Azure services covered.

## Refereces

- [Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/synapse-link)
- [Dataverse](https://docs.microsoft.com/en-us/power-apps/maker/data-platform/export-to-data-lake)
- [SQL](https://docs.microsoft.com/azure/synapse-analytics/synapse-link/sql-synapse-link-overview)

## Azure Synapse Link for Cosmos DB architecture

![](media/cosmodb-reference-architecture.png)

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

![](media/Synapse%20Link%20for%20Cosmos%20DB/cosmosdb-reference-architecture.png)

## [Lab 2 - Azure Synapse Link for Azure SQL Database](Labs/Synapse%20Link%20for%20SQL/Synapse%20Link%20for%20Azure%20SQL%20Database.md)

In this lab, you will walk through a complete end-to-end scenario to analyze large operational datasets while minimizing the impact on the performance of mission-critical transactional workloads.

[Azure Synapse Link for Azure SQL](https://docs.microsoft.com/en-us/azure/synapse-analytics/synapse-link/sql-synapse-link-overview) is an automated system for replicating data from your transactional databases (currently in both SQL Server 2022 and Azure SQL Database) into a dedicated SQL pool in Azure Synapse Analytics.

## Page Navigator

[Index: Table of Contents](Index.md)

[Next: Lab 0 - Deploy](Labs/Deploy/Readme.md)
