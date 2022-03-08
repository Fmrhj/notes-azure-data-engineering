# Optimize Azure Data Solutions

## Data partitioning Bottlenecks

Vertical
- column wise

Functional
- business related columns are grouped together

Design considerations
- Balance data distribution
- Balance workload distribution
  - Queries shall divide into all partitions
- Minimize cross-partition data access or join operations
- Replicate static reference data
- Prefere eventual consistency
- Replicate partitions


## Data Lake

- Data Ingestion considerations
  - Storage hardware
  - High speed internal network
  - Fast network connection b/w on-premise and cloud
- Parallel read/write
  - e.g. Data Factory parallel copies settings
- Structure your data set
  - File size vs number of files
    - File size b/w 256MB to 100GB
  - Folder structure
    - e.g. Dataset YYYY/MM/DD/datafile_YYYY_MM_DD_HH_MM.tsv
  - Same region (Data Lake and other services in the same region)
  - Batch data

## Stream Analytics

- Streaming Units (SUs)
  - Processing power (CPU and memory) allocated to your stream analytics job
  - ASA jobs perform all processing in memory
  - If SU% utilization is low and inputs events get backlooged
  - MS recommeds setting an alert on 80% SU Utilization
  - The best practice is to start with 6 SUs for query with the keyword `PARTITION BY` (Scale)
  - Complex query logic could have high %SU utilization even when it is not continuously receiving input events
- Parallelization
  - partition help to divide data in subsets
  - this would be based on partition key
  - Inputs are already partitioned, outputs need to be partitioned

```sql
SELECT *
INTO output
FROM input
PARTITION BY DeviceId
INTO 10
```

## Synapse

- Maintain Statistics
  - Automatically detect and create statistics on columns
  - `AUTO_CREATE_STATISTICS`
  - Update statistics of more relevant columns like date (or columns used in join, where and group by clause)
- Polybase
  - ADF or BCP can be used for small load
  - PolyBase is best choice for large volume of data
  - MPP architecture
  - CTAS or `INSERT INTO`
- Hash distribution large tables
  - By default is Round Robin distribution
  - Small tables joins - RR is fine
  - Big tables join - use hash distribution
- Do not over partition
  - Too many partition can slow down a query
  - Partition should have more than 1 million rows
  - 60 partition by default
- Use the smallest possible data type for a column
  - super important for char or varchar type coluumns
  - use varchar instead of nvarchar
- Scaling
  - Before you perform a heavy data loading or transformation operation
  - During peak business hours
- Pausing or resuming compute
  - Storage and compute are separate
  - transaction cancel

## Manage Data Lifecycle

- Blob Access Tier
  - Hot
    - frequent access. Lowest access cost. Highest storage cost
  - Cool
    - lowest storage cost, higher data access cost
  - Archive
    - lowest storage cost, highest data retrieval cost. Data is offline.
    - You have to move data first to cool or hot tier.