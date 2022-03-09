# Implement a no-sql datastore

## No-SQL Offerings by Azure

### Data Lake

A data lake is a big container for data. In contrast to data marts, where data is structured and organized, a data lake is a big dump where data can be stored.

A data lake enable first data loading, e.g. in raw format, and then to think about possible uses of those data.

### Azure Data Lake Gen1

Legacy systems at that moment:
- Built on top of hadoop (HDFS)
- Fault tolerant file system
- Runs on commodity hardware
- Principles around MapReduce, Pig, Hive, Spark
- Cloud storage:
  - Processing: easy to optimize processing by increassing virtual CPUs and RAM
  - Storage: different requirements

### Azure Data Lake Gen2

- It takes the benefits of using Azure Blob Storage
  - Large cloud storage
  - Optimized for storing big amounts of unstructured data
  - General purpose
  - Cost efficient
  - It provides multiple tiers
- Leverages also the good features of Data Lake Gen1
- It is the de facto recommendation from Azure for big data storage

| Azure Blob Storage                    | Azure Data Lake Storage Gen2                                             |
|---------------------------------------|--------------------------------------------------------------------------|
| General purpose                       | Optimized for big data                                                   |
| Container based                       | Hierarchical namespaces on Blob Storages                                 |
| Available in every Azure region       | Available in every Azure region                                          |
| Local and global redundancy           | Local and global redundancy (in contrast to Gen1, only local redundancy) |
| It has a processing performance limit | Supports a subset of Azure Blob storages features                        |
|                                       | Provides multiple Azure integrations                                     |
|                                       | **Compatible with Hadoop**                                               |

With Gen2 you get:
- Many Azure Blob Storage Features
- Hierarchical Namespace
- Blob and Azure Data Lake Service (ADLS) API

## Authentication

- Storage Account Keys
  - Password for the storage account (read, write, delete and update)
  - You can set up to two keys
  - Old way of authentication and not recommeded to use in production
- Shared acces signature (SAS) token
  - Ephemeral tokens
  - Contains permission like start and end time
  - Azure does not track these tokens after creation
- Azure Active Directory
  - RBAC method to grant privileges to different groups/users in Azure AD (e.g. Owner, Contributor, Reader)
- Acces control list (ACL)
  - specifies the read, write, execute on a folder level
- Firewalls and Virtual Networks
  - Every object in the Storage has an open endpoint. 
  - Access can be restricted and lock downed to a certain network
  - The other authenticaton methods will be also valid on top of these rules. E.g. a SAS token could be valid, but if the virtual network is not authorized, the request will be denied

## Data continuity and availability

High availability (HA): service requirement
- Making a service available within a region
- No expected data loss

Disaster recovery (DR): some incident will affect the services
- Recovery when a certain region goes down
- Typically some data loss

These two concepts must be adjusted since one affect the other.

### Azure Storage Options

| Outage scenario                                                                            | LRS                | ZRS                | GRS/RA-GRS                       | GZRS/RA-GZRS                      |
|--------------------------------------------------------------------------------------------|--------------------|--------------------|----------------------------------|-----------------------------------|
| A node within a data center becomes unavailable                                            | :white_check_mark: | :white_check_mark: | :white_check_mark:               | :white_check_mark:                |
| An entire data center (zonal or non-zonal) becomes unavailable                             | :x:                | :white_check_mark: | :white_check_mark:               | :white_check_mark:                |
| A region-wide outage occurs in the primary region                                          | :x:                | :x:                | :white_check_mark:               | :white_check_mark:                |
| Read access to the secondary region is available if the primary region becomes unavailable | :x:                | :x:                | :white_check_mark: (with RA-GRS) | :white_check_mark: (with RA-GZRS) |

*source: [storage redundancy](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy)*

If a region is unavailable we must initiate a manual failover. This is possible with the *prepare to failover* API.

> Azure Storage Outages: according to the Azure Service health dashboard see in case of:
> LRS or ZRS: wait for recovery
> GRS or RA-GRS or GZRS or RA-GZRS: manual failover, copy data from secondary to some other region (use AzCopy, PowerShell, Azure Data movement library, etc.)

### Cosmos DB Options

Cosmos includes the automatic failover. For this we need to maintain a list of priority regions.

**Single Region**
- Data within a container is durably commited by a majority of replica members within the replica set
- Availability Zone support - replicas are replaced across multiple zones within a given region

**Multi Region - Multi write**
- Regional failovers are instantaneuous and don't require any changes from the applications

**Multi Region - Single write (read region outage)**
- No changes are required in your application code
- The impacted region is automatically disconnected and will be marked offline
- The Cosmos DB SDKs will redirect read calls to the next available region
- When the impacted read region is back online it will automatically sync with the current write region and will be available again to serve read requests

**Multi Region - Single write (write region outage)**
- Manual failover
- automatic failover enable - automatically promote a secondary region (no action required)
- When impacted region back only - lost data can be recovered using conflict feed
- You can switch back to the recovered region as the write region using Cosmos APIs

> Data corrupted and rolled over? Use backup and restore. 
> Default:  
>   - Interval: every 4 hours
>   - Retention: 8 hours
>   - Interval and retention can be changed but max. 2 backups are possible
> How to restore? Raise a ticket to support team. Customised solutions are also possible
