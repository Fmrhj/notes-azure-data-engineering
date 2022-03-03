# Azure Data Technologies

## Storage Account

When you:

- need low cost, high thorughput data store,
- need to store No-SQL data,
- do not need to query the data directly (no ad hoc query supported),
- need to archive or have relatively static data

## Data Lake Store

When you:

- need low cost, high thorughput data store,
- unlimited storage for no-sql data
- do not need to query the data directly (no ad hoc query supported - this changes with v2),
- need to archive or have relatively static data
- use Databricks, HDInsight and IoT and need a data store.

## Azure Databricks

- Eases deployment of Spark based cluster.
- Enables fastest processing of Machine Learning solutions.
- Enables collaboration between data engineers and data scientists.
- Provides tight enterprise security integration with Azure AD.
- Integration with other Azure Services and Power BI.

## Azure CosmosDB

- Provides global distribution for both structured and **unstructured** data stores
- Millisecond query response time
- 99.999% Availablity
- Worlwide elastic scale of both storage and throughput
- Multiple consistency levels to control data integrity and concurrency

## Azure SQL DB

When you

- require a relational data store,
- need to mange **transactional** worloads,
- need to manage high volume on inserts and reads,
- need to a service that requires high concurrency,
- require a solution that can scale elastically.
- 
## Azure Data Warehouse

When you

- require a relational data store,
- need to manage analytical workloads,
- you need low cost storage,
- when you need the ability to pause and restart compute (compute load)
- need a solution, you can scale elastically

## Azure Stream Analytics (ASA)

When you

- require a fully managed event processing engine,
- require a temporal analysis of streaming data
- need support for analyzing IoT streaming data
- analyzing application data from Event Hubs.
- It brings a Strem Analytics Query Language (Similar to Google's Dataflow).

## Azure Data Factory

When you want to

- create data pipelines,
- orchestrate the batch movement of data,
- connect a wide range of data platforms and sources,
- move data from A to B,
- enrich your data,
- integrate with SSIS packages,
- monitor the state of data pipelines

## Azure HDInsight

- When you need low cost, high thorughput data store.
- Stores No-SQL data.
- Provides a Hadoop Platform as a Service approach.
- You use Hadoop, Hbase, LLAP or Kafka and you need a data store.
- Eases deployment and management of Hadoop clusters.

## Azure Data Catalog

- Registers data stores
- Multi user approach to documentation
- Provides annotation of data sources with descriptive metadata
- Fully managed cloud service
- Helps users to understand data
- Similar to [Apache Atlas](https://atlas.apache.org/#/)