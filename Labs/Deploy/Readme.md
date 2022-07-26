# Deploy the solution

In this section you will provision all Azure resources required to complete labs. We will use a pre-defined ARM template with the definition of all Azure services used to ingest, store, process and visualize data.

## Azure services provisioned for the workshop

Some of the Azure services provisioned require globally unique name and a “-suffix” has been added to their names to ensure this uniqueness.

> [!CAUTION]
> Make sure you deploy all the resources in the correct order

| Order | Azure Service | Name | How to |
| :--:  |:----          |:----- |:----- |
| 1     | Resource Group | synapse-link |[Create a Resource Group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups)
| 2     | Storage Account Data Lake Gen 2| synapsedatalake*suffix* |[Create an Azure Data Lake Storage Gen2 account](https://docs.microsoft.com/en-us/azure/storage/blobs/create-data-lake-storage-account)|
| 3     |Anomaly Detector| anomaly-detector-*suffix* |[Create an Azure Cognitive Service Anomaly Detector](https://docs.microsoft.com/en-us/azure/cognitive-services/anomaly-detector/how-to/deploy-anomaly-detection-on-iot-edge#create-an-anomaly-detector-resource)|
| 4     |Azure Synapse Analytics |  synapse-link-*suffix* |[Create an Azure Synapse Analytics](https://docs.microsoft.com/en-us/azure/synapse-analytics/quickstart-create-workspace)|
| 5     | Azure Cosmos DB| cosmosdb-link-*suffix* |[Create an Azure Cosmos DB account](https://docs.microsoft.com/en-us/azure/cosmos-db/sql/create-cosmosdb-resources-portal#create-an-azure-cosmos-db-account)|
| 6     |Azure Key Vault|  key-vault-*suffix* | [Create an Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/general/quick-create-portal)|

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
