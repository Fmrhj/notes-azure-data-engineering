# Implement a no-sql datastore

## No-SQL Offerings by Azure

- Azure Storage
  - Blob: any type of data. Cheap but slow
  - Table: data that can be broken down into fields -> Cosmos DB is a premium version of this
  - File (not necesarily no-sql) SMB-Protocol: you can attach several VMs to this file system and thus share data.
  - Queue (not necesarily no-sql)
- Data Lake
  - For large quantities of data
  - Very big (maybe infinite) container of data
- Cosmos DB
  - Cassandra API: wide column
  - Table API
  - SQL / Mongo DB API

### Azure Storage

- Highly Available
  - Disk Options
- Durable
- Secure (all is encrpyted)
  - Even possible to use own encryption key
  - Possible to put in a virtual network
- Scalable
  - Meant to store big data
- Cost effective
  - Different tiers: flexible cost
  - Good for archiving
- Accessible via HTTPS (REST) and other SDKs
  - UI: Azure Storage Explorer
  - Other Tools like AzCopy

### Options

- Performance:
  - Standard: HDD
  - Premium: SSD (solid state drive), replication
  - Differences with availability of block, file storage, etc. In conjunction with Account type, e.g. Premium
- Account type:
  - General Purpose v2
  - v1: can convert to v2. Does not support blob access tiers
  - Blob Storage: only supports Blobs and Blob Access Tiers. It is recommended to use Gen Purpose v2, since this is a subset of that.
- **Replication** (6 options) Protect data from unplanned events which make a down of the system. Options:
    > Each region has many availability zones.
  - Locally-redundant storage (LRS)
    Data is copied in the same availability zone.
  - Zone-redundant storage (ZRS)
   > Data is copied across many availablity zones in the same region
  - Geo-redundant storage (GRS)
    > Data is copied in separate regions
  - Geo zone-redundant storage (GZRS)
  > Copied across many availability zones in one region **and also** in another region (in the same availability zone). This data is, however, not directly available to the application.
  - Read access geo-redundant storage (RA-GRS)
  > Data replicated across regions and, without failure, can access data in another region (read access).
- Blob Access Tier:
  - Hot: Highest storage cost, lowest data access cost.
  - Cool: Lowest storage cost. Higher data access cost.
  - Archive: Lowest cost. Highest data retrieval cost. Data is offline.
- Networking:
  - Access (connectiviy method):
    - Public endpoint (all networks)
    - Public endpoing (selected network: Azure Vnet)
    - Private endpoint (IP Address space)
  - Network routing:
    - Microsoft (default)
    - Internet routing
- Advanced:
  - Security
    - Security transfer (yes/no)
    - Block public access
    - Minimum TLS
    - Azure Files (Large files: yes/no up to 100 TB)
    - Data Lake Storage Gen2(yes/no)
      - Allows nested namespaces

### Azure Blob Storage

> Block Blob Storage:
    - Better performance for high transaction rates, low storage latency
    - Backed by SSDs
    - Block and Append Blobs only

- Cheapest way to store data in Azure
- HDFS and blob storage REST API
- Blob Types
  - Block Blob: one blob is made from many blobs (good for optimizing data storage)
  - Append Blob (ideal for logs)
  - Page blob
    - (VM disks and databases)
    - Frequent random read/write applications

#### Use Cases

- Only basic storage is needed
- Data is unstructured
- Data that is older or not used as much
- Budget is critical

#### Advantages

- Cheap (~2 ct/GB)
- Simple to setup
- No configuration
- Does not require powerful computing to manage

#### Limitations

- Not optimized for performance: use Data Lake (built on top)
- No seach tools (or query)
- You are responsible of replication and sync

> Acquire lease: no one else can change the file
