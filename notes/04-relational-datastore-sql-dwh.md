# Relational datastores

## Modern Data Warehouses

Modern data architectures use

- Data Ingestion Services
- Data Transforming Services
- Data Processsing
- Data Serving
- Data Visualization

For that Azure has different services. Today there is one unifying servuice that provides end-to-end analytics. This service is called **Azure Synapse**.

Azure Synapse emcompasses:
- Azure Data Factory
- Azure Data Lake Gen2
- Azure Databricks
- Azure SQL Data Warehouse (Dedicated SQL Pool)
- PowerBI

## Azure SQL Data Warehouse (Dedicated SQL Pool)

Two possible ways to deploy:

- Standalone service 
- As part of a Azure Synapse Analytics Workspace

> If created as standalone service, it can be connected to Synapse afterwards as well.

## Azure Synapse Analytics Studio

- Ingest: Copy data jobs
- Explore and Analyse: Data / Develop (SQL scripts and Notebooks). Also link PowerBI
- Integrate: Pretty much Azure Data Factory (ADF). Build data pipelines
- Monitor: Like ADF monitoring. It also includes the status of SQL and Spark jobs.
- Manage: also like ADF configuration tab. Panel to manage connections, resources, integrations (triggers, integration runtimes), security and git.

> For scripts and spark notebooks, publish it to the workspace so that you can continue working with it.

## Analyzing Data with Spark Pool

You can run complex queries, take data from a dedicated SQL pool and write it back.

## Azure Synapse: MPP Architecture

The Massive Parallel Processing (MPP) aims to process as much parallel operations as possible. Its elements are:

- **Control Node**: Application or user connection. Frontend
- **MPP Engine**: Runs on the control node to optimize and coordinate parallel queries
- **Compute Nodes**: Computational power. Dsitrbuted map to compute nodes for processing. 
  - These are scaled using data warehouse unit (DWU). These amount shall be set. CPU, memory and I/O are bounded to that. If I increase the DWU by a factor o *n*, an improvement of *n* to load or process data can be expected. 
  - DMS: Data transport technology that coordinates data movement between compute nodes.
- **Azure Storage**: Storage layer.

## Azure Storage and Distribution: Sharding patterns

- Storage and Compute are completely separated. 
- The data is stored distributed. 
- Rows are stored across 60 distribution which are run in paralell
- Each compute node manages or more of these 60 distributions

### Sharding patterns

- Hash: quicker for joins and aggregations. Each row belong to one particular distribution. This is done by means of a hash function. Good for fact tables.
- Round robin (default): generally used to load tables. Distribute data evenly across the table. Each row is distirbuted evenly. Record 1 -> Distribution 1, Record 2 -> Distribution 2, etc. It is good when there is no obvious joining key.
- Replicated: complete copy on each compute node. Suitable for small tables

```sql
CREATE TABLE (
 /*Column defnition */
)
WITH (
  CLUSTERED COLUMNSTORE INDEX
  DISTRIBUTION = REPLICATE
)
-- or 
WITH (
  CLUSTERED COLUMNSTORE INDEX
  DISTRIBUTION = ROUND_ROBIN
)
-- or 
WITH (
  CLUSTERED COLUMNSTORE INDEX
  -- column name for the hash function shall be provided
  DISTRIBUTION = HASH((P&L))
)
```

### Good hash key

- Will distribute evenly
- Used for grouping
- Used as a join condition
- It is not updated
- Has more than **60 distinct values**: for evenly distribution

> A round robin method will evenly distribute data but it would not be the most efficient since you would have to look in other partitions considering a certain join operation.


## Data Types

As a general principle, we should use the smallest data type for our data that will fit for our use case.

Some hints:
- Avoid defining all character columns to a large default length
- Define columns as VARCHAR rather than NVARCHAR if you do not need Unicode

Data Types are not only good for storing and saving disk space but also for moving data.

### Table Types

**Clustered columns store**

- Updateable primary storage method
- Great for read-only

**Heap**
- Data is not in any particular order
- Great for non-ordinal data

**Clustered Index**
- an index that is physically stored in the same order as the data being indexed

## Best practices for fact and dimensional model

- Fact tables:
  -  Larges ones are better as Column stores
  -  distributed through hash key as much as possible (as long it remains an even distribution of the data)
  -  Partitioned only if the table is large enough to fill up each segment (60 distributions x n partitions)
- Dimensions tables:
  - Can be hash distributed or round-robin if there is no clear candidate join key
  - Column store for large dimensions
  - Heap or clustered index for small dimensions
  - Add secondary indexes for alterante join columns
  - Partitioning is not recommended (since these are generally small tables, i.e. not enough rows to fill each segment)

## Loading methods

- Single client loading methods
  - SSIS
  - Azure Data Factory
  - BCP (bulk copy)
  - Can add some parallel capabilities but are bottlenecked at the control node
  - All these methods interact directly with the control node, since it receives the connection and orchestrates the queries, e.g. optimizes the queries.
- Parallel readers loading methods
  - PolyBase: reads from Azure Blob storage and loads the contents into Azure SQL DW. This bypasses the control node and loads directly into the compute nodes
  - It leverages the MMP architecture from Synapse
  - It can load efficient data formats like parquet, RC, ORC, etc.

> The readers will not be able to read a compressed file. The file needs to be decompressed beforehand.

## Scaling DWH

- SQL DW allows us to scale or pause dynamically
- DWU are CPI, memory and I/O represented into units of compute
- Increasing DWUs:
  - Increase query performance
  - Also increases max. number of concurrent queries

## Azure SQL vs Azure SQL DW

| Azure SQL DB                  | Azure SQL DW                           |
|-------------------------------|----------------------------------------|
| OLTP/CRUD                     | OLAP / querying and reporting          |
| SMP: simetric multiprocessing | MPP: massive parallel processing       |
| Vertical scale                | Horizontal scale (behind the curtains) |
| It does not provide PolyBase  | It provides Polybase                   |

## Dynamic Data Masking (DDM)

Built-in functions in order to mask data.

- Random Number Function
- Custom String Function: you can define what it is exposed
- Email function: exposes first letter and replaces the domain
- Credit Card function: only last 4 digits
- Default function (full mask of the data)

> Add masking rules: under `Security/Dynamic Data masking`. 
> The data will be however displayed as result of a query containing that data, regardless of the masking. Therefore this method should not be the primary security method for the data.

How to DDM:
- Powershell
- Portal
- REST

## Encrypt data at rest and in motion

- At rest: keys are stored in Azure Key Vault (256 bit AES encrypton). By default all storage services are encrypted at rest. This is managed by Microsoft. You can also provide your own key (e.g. from a KeyVault).
- At transit: 
  - encrypted via SSl/TLS protocols.
  - Utilize site-to-site VPN or point-to-site VPN connections
  - storages services can be configured to require secure transfer