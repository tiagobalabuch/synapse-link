# Deploy the solution

In this section you will provision all Azure resources required to complete labs. We will use a pre-defined ARM template with the definition of all Azure services used to ingest, store, process and visualize data.

## Azure Services used in this lab

There will be a few resources to support an Azure Synapse link:

- An Azure Resource Group
- An Azure Synapse Workspace
- An Azure Synapse SQL Pool
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
  - Perform Sales forecasting using Azure Synapse Link and Azure Machine Learning
- PySpark Notebook to:
  - Ingest stream and batch data into CosmosDB containers
  - Fetch data from CosmosDB
  - Join dataset together
  - Perform Anomaly Detection using Azure Synapse Link and Azure Cognitive Services on SynapseML

## Azure services provisioned for the workshop

Some of the Azure services provisioned require a globally unique name and a “-suffix” has been added to their names to ensure uniqueness.

> [!CAUTION]
> Make sure you deploy all the resources in the correct order

> [!NOTE]
> You have to deploy all resources manually.
> One click deploy will come later

| Order | Azure Service | Name   | Pricing Tier    | How to |
| :--:  |:----          |:----- | :----   |:----- |
| 1     | Resource Group | synapse-link |   | [Create a Resource Group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups)
| 2     | Storage Account Data Lake Gen 2| synapsedatalake*suffix* |    |[Create an Azure Data Lake Storage Gen2 account](https://docs.microsoft.com/en-us/azure/storage/blobs/create-data-lake-storage-account)|
| 3 | Storage Account | synapselinkml*suffix* | | [Create a storage account](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-create?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-portal)|
| 3     |Anomaly Detector| anomaly-detector-*suffix* | Free  |[Create an Azure Cognitive Service Anomaly Detector](https://docs.microsoft.com/en-us/azure/cognitive-services/anomaly-detector/how-to/deploy-anomaly-detection-on-iot-edge#create-an-anomaly-detector-resource)|
| 4     |Azure Synapse Analytics |  synapse-link-*suffix* | |[Create an Azure Synapse Analytics](https://docs.microsoft.com/en-us/azure/synapse-analytics/quickstart-create-workspace)|
| 5     | Azure Cosmos DB| cosmosdb-link-*suffix* | 4000 RU/sec |[Create an Azure Cosmos DB account](https://docs.microsoft.com/en-us/azure/cosmos-db/sql/create-cosmosdb-resources-portal#create-an-azure-cosmos-db-account)|
| 6     |Azure Key Vault|  key-vault-*suffix* | Standard |[Create an Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/general/quick-create-portal)|
| 7 | Log Analytics Workspace | log-analytics-*suffix*| | [Create a Log Analytics workspace](https://docs.microsoft.com/en-us/azure/azure-monitor/logs/quick-create-workspace?tabs=azure-portal)|
| 7 | Application Insights | application-insights-*suffix*||[Create an Application Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/app/create-new-resource)
| 7 | Azure Machine Learning |aml-workspace-*suffix*  |  |[Create as Azure Machine Learning Workspace](https://docs.microsoft.com/en-us/azure/machine-learning/quickstart-create-resources)|
| 8 | Azure SQL Database | sql-link-*suffix* | S3 100 DTU |  [Create a single database - Azure SQL Database](https://docs.microsoft.com/en-us/azure/azure-sql/database/single-database-create-quickstart?view=azuresql&tabs=azure-portal)
| 9 | Azure Synapse Analytics Dedicated SQL Pool  | DW | DW100c |[Create a dedicated SQL pool using Synapse Studio](https://docs.microsoft.com/en-us/azure/synapse-analytics/quickstart-create-sql-pool-studio)|

### Azure Machine Learning deployment

When deploying a Azure Machine Learning make sure to use:

- **Storage account**: synapselinkml*suffix*
- **Key Vault**: key-vault-*suffix*
- **Application Insights**: application-insights-*suffix*

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

![cosmosdb-database-container](../../media/Deploy/cosmosdb-database-containers.png)

### Create a Spark pool

Apache Spark is a parallel processing framework that supports in-memory processing to boost the performance of big data analytic applications and
An Apache Spark pool provides open-source big data compute capabilities.

1. Start creating two Apache Spark pool into you Azure Synapse Analytics workspace. [Create a new serverless Apache Spark pool](https://docs.microsoft.com/en-us/azure/synapse-analytics/quickstart-create-apache-spark-pool-portal).

- **Apache Spark pool name**: demoaml
- **Node size family**: Memory Optimized
- **Node size**: Medium (8 vCores / 64 GB)
- **Autoscale**: Enable
- **Automatic pausing**: Enable
- **Apache Spark**: 2.4

Unfortunately, Spark 2.4 is a prerequisite from [train a machine learning model](https://docs.microsoft.com/en-us/azure/synapse-analytics/machine-learning/tutorial-automl#prerequisites)

- **Apache Spark pool name**: demo
- **Node size family**: Memory Optimized
- **Node size**: Medium (8 vCores / 64 GB)
- **Autoscale**: Enable
- **Automatic pausing**: Enable
- **Apache Spark**: 3.2

### Create a Dedicated SQL pool

 A dedicated SQL pool offers T-SQL based compute and storage capabilities.

1. Start creating a Dedicated SQL pool into you Azure Synapse Analytics workspace. [Create a dedicated SQL pool using Synapse Studio](https://docs.microsoft.com/en-us/azure/synapse-analytics/quickstart-create-sql-pool-studio).

- Dedicated SQL pool name: **DW**
- Performance level: **DW100c**

> [!WARNING]
> Immediately after you deploy a Dedicated Pool, you must pause it to save money. We will only use it later.

[Pause and resume compute in dedicated SQL pool via the Azure portal](https://docs.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/pause-and-resume-compute-portal)

### Granting permission for Azure Synapse Analytics

Now, you have to grant permission for Azure Synapse Analytics workspace to access the Azure Machine Learning Workspace.

[Give MSI permission to the Azure ML workspace](https://docs.microsoft.com/en-us/azure/synapse-analytics/machine-learning/quickstart-integrate-azure-machine-learning#give-msi-permission-to-the-azure-ml-workspace)

1. Open your Azure Machine Learning resource
2. Navigate to 'Access control (IAM)' section in the left panel and then click '*+ Add*'.
![azure-ml-IAM](../../media/Deploy/azure-ml-IAM.png)
3. Select '*Add role assignment*'
4. Select '*Contributor*' role anc click '*Next*'
![azure-ml-IAM-role](../../media/Deploy/azure-ml-IAM-role.png)
5. Select '*+ Select members*' and find out the Azure Synapse Analytics Workspace (synapse-link-*suffix*) you created previously.
![azure-ml-IAM-member](../../media/Deploy/azure-ml-IAM-member.png)
6. Click '*Review + assign*'

> [!IMPORTANT]
> We will create a linked services during the lab.

## Deploying an Azure logical SQL Server

In Azure SQL Database a [server is a logical](https://docs.microsoft.com/en-us/azure/azure-sql/database/logical-servers?view=azuresql&tabs=portal#create-a-blank-server) construct that acts as a central administrative point for a collection of databases. At the server level, you can administer logins, firewall rules, auditing rules, threat detection policies, and auto-failover groups.

We have to first create a logical SQL server

1. In Azure Portal click '*+ Create a resource*'. In the left panel click "Database"
2. Then click SQL server (logical server)
   ![azure-logical-sql-server-menu](../../media/Deploy/azure-logical-sql-server-menu.png)
3. Resource group: **synapse-link**
4. Server name: **sql-link-*suffix***
5. Location: **choose the nearest location for you**
6. Authentication method: **Use SQL authentication**
7. Server admin login: **sqladmin**
8. Password: **"WhatEver@Iwant123456"**
![azure-logical-sql-server-basic-settings](../../media/Deploy/azure-logical-sql-server-basic-settings.png)
9. Networking
    1. Allow Azure services and resources to access this server: **Yes**

10. Click "*Review + create*" and then "*Create*"

Once the deployment is finished, it's time to import our database. We are going to use a BACPAC file to load our data.

> [!WARNING]
> Make sure you follow the instruction bellow, otherwise you won't be able to continue.

Download the RetailDB BACPAC file [here](https://synapselinkdemoworkshop.blob.core.windows.net/synapse-link/sql/RetailDB.bacpac).

Follow the instruction to [import a BACPAC file to a database in Azure SQL Database](https://docs.microsoft.com/en-us/azure/azure-sql/database/database-import?view=azuresql&tabs=azure-powershell)

- Upload the file to your storage account **synapsedatalake*suffix***
- Pricing Tier: **Standard S3**
- Database name: **RetailDB**
- Authentication type: **SQL Server**
- Server admin login: **sqladmin**
- Password: **"WhatEver@Iwant123456"**

## Page Navigator

[Index: Table of Contents](../../Index.md)

[Prev: Azure Synapse Link Workshop](../../README.md)

[Next: Lab 1 - Azure Synapse Link for Cosmos DB](../Synapse%20Link%20for%20Cosmos%20DB/Synapse%20Link%20for%20Cosmos%20DB.md)