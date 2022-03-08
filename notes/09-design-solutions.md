# Designing an Azure Data Solution

## Data Types

- **Structured**
  - Highly organized
  - It has an schema
  - Example: DB, DWH, ERP, CRM
  - *Schema-on-write*:
    - Highly precise schema that is define before writing
  - Difficult to make changes. It involves schema adaption
  - Extract Transform Load (ETL)
- **Semi-structured data**
  - Is not that well organized. Structure is not strictly enforced
  - Example:
    - csv, xml, json
  - Easy to make changes on schema
  - *Schema on-read*
- **Unstructured data**
  - Examples: Videos, images, emails, music
  - 90% of all data is unstructured
  - Highly flexible to accept new changes to the data
  - Vast assortment of data types and growing every day
- **Streaming data**
  - Data in continous flow
  - Example: media, satellite, IoT
  - Streaming data analysis
    - Batch
      - we store data ant then we analyze it
    - Real-time
      - the data is analyzed during gathering

## Data storage types

### Categories

- Structure of data
- Types of operations on data

### Types

- *Relational database management systems (RDBMS)*
  - Organize data as a series of two dimensional tables with rows and columns
  - Schema-on-write
  - normalized (in best cases)
  - relationship are enforced in form of constraints
  - **Operations**
    - Structured Query Language (SQL)
    - ACID (Atomic, Consistent, Isolated, Durable)
- *Key Value Stores*
  - Each data value associated with an unique key
  - Scalable
  - No relationship between entities
  - **Operation**
    - Support single insert/delete operations
    - Highly optimized applications performing simple lookups
  - Ex. Cosmos, Azure Cache for Redis
- *Document DBs*
  - Similar to key values. Instead of a value, we have a complete document
  - Document data is semi-structured
  - **Operation**
    - Individual documenta are retrieved and written as single block
    - Data requires index on multiple fields
- **Graph DBs**
  - Stores two types of information: nodes and edges
  - Nodes are similar to table rows or JSONB documents
  - Complex relationships between data items
  - *+Operation**:
    - Efficiently perform queries across the network of nodes 
    - Data requires index on multiple items
  - Ex. Cosmos DB Gremlin API, SQL Server
- **Column-family DBs**
  - Organizes data into rows and ciolumns
  - denormalized approach to structuring sparse data
  - Each column family holds a set of columns that are logically related together
  - **Operation**:
    - Read and write operations for a row are usually atomic with a single column-family
    - Update and delete operations are rare
  - Ex. Azure Cosmos DB Cassandra API, HBase in HDInsight
- **Data Analytics**
  - MPP Architecture
  - Usually denormalized star or snowflake schema
  - Consisting of fact and dimension tables
  - Operation
    - Data Analytics
    - Enterprise BI
  - Azure Services
    - Synapse
  - **Object storage**
    - Optimized for storing and retrieven large binary objects
    - Identified by key
    - Exemple:
      - Azure Blob Storage
      - Azure Data Lake Storage Gen2
  - **File**
    - Enable file sharing across networks
    - Accessible with standard I/O libraries
    - Azure Files
  - **Time Series**
    - Azure TIme Series Insights
  - **Search Engines**
    - Azure Search

## RTO and RPO

| Recovery Time Objective (RTO)                                                                                                                | Recovery Point Objective (RPO)                                                 |
|----------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------|
| The maximum acceptable time that an <br>incident application can be unavailable after<br>an incident                                         | The maximum duration of data <br>loss that is acceptable during a <br>disaster |
| E.g. a RPO of 90 minutes, you must be able to<br>restore the application to running state within <br>90 minutes from the start of a disaster | E.g. one standalone DB with hourly backups<br>provides an RPO of 60 minutes    |
| If you have a very low RTO, you might need <br>a warm standby running to protect against a<br>regional outage                                | If you require a lower RPO you would<br>need to design accordingly             |

## Lambda Architecture

- Real-time layer (hot path)
- Batch layer (cold path)

Example 1:

1. IoT Devices
2. Broker (Azure IoT Bus)
3. ASA
4. Lambda gable
   1. Hot: Action with Azure Functions
   2. Store for later usage (Cosmos, SQL DB, Service Bus, Azure Data Lake)

Example 2 (Data bricks):

1. Azure IoT Bus or ADF as ingestion layer
2. Raw Batch Data can be processed in Data Bricks
3. IoT Events can be processed in Azure Event Hub and then in Databricks Stream Processing
   1. These data can be used to train ML models
   2. These models or signals can be directed to Azure Service Bus
4. After that processed data can be stored in a storage like Azure Data Lake Gen2
5. Transformed Batch data can be served and analyzed further in Synapse
6. Present and serve