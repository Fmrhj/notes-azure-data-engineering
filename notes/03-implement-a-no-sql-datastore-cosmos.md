# Implement a no-sql datastore

## No-SQL Offerings by Azure

### Cosmos

#### History

- Global distributed databases were not easy to setup.
- SQL Server was not yet at the point to solve this problem.
- 2015: Azure Document DB. It stored data in form of JSON. One could run SQL against it.
- 2017: Cosmos DB. Globally distributed and scalable.

#### Description

- Globally distributed multi model database service for **mission critical applications**
- Fully managed
- Database as a service
- Serverless architeture.
- Protected via a high SLA (five 9s) while it is globally distributed. Big commitmment
- All data is encrypted
- No schema or index management (this is integrated and managed by Cosmos)
- More like a SaaS.
- Multi model: it handles different No-SQL models. Not only JSON or document, but also columnar data models.

#### Use Cases

- Highly distributed
  - Gaming
  - IoT (e.g. Azure IoT Hub -> Databricks)
  - Retail (e.g. Browser -> Azure Web App -> CosmosDB)
- Highly available
- Easy to deploy

### Connections

- Cosmos DB SQL and MongoDB API
- Cosmos DB Table API
  - Key-Value Store
  - Premium offering for Azure Table Storage
  - Existing Customers from Azure Table Storage can migrate easily
  - Row value can be simple like number or string
  - Row cannot store objects
- Cosmos DB Cassandra API
  - Wide column NO SQL DB
  - Name and format of column can vary from row to row
  - Simple migrate your Cassandra application to Cosmos
  - Interaction:
    - Cassandra based tools
    - Data Explorer
    - Programmatically, using SKD
- Cosmos DB Gremlin API (for Graph DBs)
  - Graph Data Model (Focused on entities)
  - Real world data connected with each other
  - Graph databases can persist relationships in the storage layer
  - Use cases:
    - Social networks
    - Recommendation engines
    - Geospatial
    - Internet of Things
  - All objects are connected with each other with a relationship

#### Analyze the decision criteria

|        | Core (SQL) | MongoDB | Cassandra | Azure Table | Gremlin |
|:--------|:---------:|:-------:|:---------:|:-----------:|:-------:|
| New projects from scratch                 | x |   |   |   |    |
|Existing MongoDB, Cassandra, Azure Table or Gremlin Data                                        |   | x | x | x  | x |
|Analysis of the relationship between data  |   |   |   |    | x |
|All other scenarios                        | x |   |   |    |   |

## Creating an Azure Cosmos DB Account

- One Account is binded to one API (e.g. Core (SQL))
- Location: can be multiple
- Account Type: Production/Non Production
- Geo-Redundancy: enable global distribution

### DB Container

> In Data Explorer: 
> ContainerId + PartitionKey

Each Cosmos **Account** has:

- One or many **databases** which have
  - One or many **containers** with:
    - Stored procedures
    - User-defined functions
    - Triggers
    - Items
    - Merge Procedures
    - Conflicts

### Measuring Performance

- Latency: How fast is the response for a given request?
- Throughput: How many requests can be served within a specific period of time? Usage is expressed in Requests Units. So Throughput is defined as Request Units / period of time.

You can reserve certain throughput (minimum throughput) for a certain container.

Exceeded throughput will be met with a HTTP 429 error.

#### Horizontal Scalable

How Cosmos DB works behind the curtain?

Container can store data. Under this container you have several VMs. Cosmos DB spins off more VMs to **scale horizontally** for a certain container.

How Cosmos DB decide which data goes to which VM?

Via the partition key. Based on the value of the partition key, the data will be divided first logically. After that these logical partitions will be mapped to **physical partitions**. This mapping is taking care by Cosmos DB.

- Partitioning: items in a container are divided into subsets called logical partitions
- Partition Key: is the value by which Cosmos organizes the data into logical divisions
- Logical partitions: formed based on the value of the partition key
- Physical partitions: internatlly, one or more logical partitions are mapped to a single physical partition (storage). Cosmos could change the mapping to a different logical partition. This is doing for an entirely logical partition.

#### Throughput

Could be define at DB or Container level.

When defined at DB level, this is shared to all containers (this is called **shared throughput**). For example if DB has 1000 RU/s, if a container tries to consume 600, there will be only 400 left for all containers in that DB.

When defined at container level is called **dedicated throughput**. Then after it will divide the throughput for all logical patitions.

#### Avoiding hot partitions

Choose a partition key for logical partitions such that there are no containers that utilize all or almost all the throughput. This is important since it will affect the query of the data.

Examples:

- Bad choice: current time
- Good choice: userID, productId

For Cosmos DB: avoid cross partition queries (fan out queries). Example: partition key is user name and you have a regular query to find a user with a certain favorite color:

```sql''
-- Example of a fan out query
SELECT * FROM users WHERE users.favorite_color = 'blue'
```

Cosmos will have to go through all logical partitions to check whether the user has favourite color  = 'blue'.

#### Composite key

You can use a composed key so that data will be distributed among much more small logical partitions. Example: userName-date

### Summmary: partiton Key

- Evenly distribute storage
  - Make sure you pick your partition key that does not result in hot sposts within your applications
  - Have a high cardinality
  - Do not be afraid of choosing a partition key that has a large number of values
  - Example UserId & ProductId
- Evenly distribute requests
  - RUs evenly distribute across all partitions
  - Review where clause of top queries
- Consider document and partition limit while designing partition key
  - Max. document size: 2MB
  - Max. logical partition size 20GB

Question Example:
Your organization is planning to use Cosmos to store vehicle telemtry data generated from millions of vehicles every second Which of the following options for your partition key will optimize storage distribution?

1. Vehicle model
2. Vehicle identification number (VIN). Ex. WDDEJ823124D

Answer: 2. Auto manufacturers have transactions ocurring throughout the year. This option will create a more balanced distribution of storage across partiton key values.
Option 1 will create a hot partition because there could be much more data instances of a particular model hence the logical partitions are not evenly distributed. Less granular.

#### Time to live (TTL)

Objects will be deleted after setting an expire time. The time to live is configured in seconds.

#### Globally distribute data

Under `Settings/Replicate data globally`.

- Ensures performance:
  - High availability within a region
  - Across regions, brings data closer to the consumer
- Business continuity
  - In the event of a major failure or natural disaster

You can also distinguish by write and read regions. Check multi-region writes.

#### Data consistency

How to prevent race conditions basically.

5 Consistency levels:

- Strong: Strong consistency, higher latency and lower availabilty.
- Bounded staleness: dirty reads possible, bounded by time and updates.
- Session: the same session will be always in sync. Other sessions could be delayed.
- Consistent prefix: Dirty reads possible but sequence maintain.
- Eventual: focused on avalability not so much on consistency. Weakast consistency, lower latency, higher availablity.

It is important the data we read is in the right order but not so much the latest updated data.

Parameters:

- Max. Lag (Time). Example 5 seconds.
- Max. Lag (Operations). Example 100 Operations.

Session is always default. This can be overriden at request level to a less consistent level (to decrease latency for instance).


## Azure CLI for COSMOS DB

```bash 
az cosmos create --name 
                  --resource-group
                  [--capabilities]
                  [--default-consistency-level {BoundedStaleness, ConsistenPrefix, Eventual,Session, Strong}] 
                  [--enable-automatic-failover {false, true}] 
                  [--enable-multiple-write-locations {false, true}] 
                  [--enable-virtual-network {false, true}] 
                  [--ip-range-filter] 
                  [--kind {GlobalDocumentDB, MongoDB, Parse}] 
                  [--locations] 
                  [--max-interval]
                  [--max-staleness-prefix]
                  [--subscription]
                  [--tags]
                  [--virtual-network-rules] 
```

**Important points**:
- Take a look on the available APIs to interact with the new database, e.g. Gremlin in case of Graph DBs.
- See whether the consistency level allows to change to a weaker level
- Remember that the changing the consistency level in bounded to a default consistency level at the time of creation

## Pricing 

Scale:
  - Manual: set a max. worload (RU/s)
  - Throughput: Azure set dynamically that value automatically

Regions:
  - The numbers of regions multiply directly the costs of the actual throughput, i.e. (N x throughout costs) with N > 1

Plans:
  - Long term plans, i.e. longer than one year, are marked as reserved capacity. This saves costs (up to 65%)