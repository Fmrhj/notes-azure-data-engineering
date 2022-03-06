# Relational datastores

## Azure SQL Databases offerings

Relational database-as-a-service in the cloud for critical workload.

- Fully managed
- Predictable performance and pricing
- Elastic pool for unpredictable workloads
- For 9s of availability (99.99% - 52 minutes per year)
- Geo replication 
- Supports existing SQL
- Easy to scale with no downtime
- Security and compliant

### Deployment options

**PaaS**
- Single DB
  - Isolated DB: its own set of assigned resources: memory and storage
- Elastic Pool
  - Fixed resources will be shared by all DBs in the pool
- Managed Instance
  - Each managed instance has its guarenteed resources
  - Easy to setup

## Single DB

### Service Tiers

**vCore**: Indenpendently scale compute and memory
- General Purpose: up to 4 TB in size
- Business Critical: up to 4TB in size
- Hyperscale: larger than 4TB

**DTU**: CPU + Memory + I/O
- Basic
- Standard
- Premium

| vCore-based                                                        | DTU-based                                                                                               |
|--------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| For single DB, elastic pool and managed instance                   | Only for single DB and elastic pool                                                                     |
| Best for customers who need flexibility, control and transparency  | Best for customers for simple and easy to configure                                                     |
| Straight-forward way to translate on-premise workload to the cloud | Might need to calculate needed DTUs                                                                     |
| Microsoft recommends vCore-based model                             | if the DTU-based purchasing model meets your performance and business requirements, it should be enough |

Converting DTU-based Model to vCored-based
- for consumption over more than 300 DTUs vCore-based may reduce costs
- You can use any Azure API with zero downtime
- Azure SQL DB managed onstance only supports vCore-based purchasing model

### Database Option Flow

1. Deployment Options
   1. Single, 
   2. Elastic or 
   3. Managed Instance
2. Purchasing model
   1. DTU
   2. vCore 
3. Service Tier
   1. General vs standard
   2. Business critical or Premium
   3. Service Tier

> Also you can use among compute generations Gen4, Gen5, etc.
> You can also allocate within *provisioned* or a *serverless* model

## Elastic Pool

An Azure SQL Elastic Pool allows you to allocate a set of shared set of compute resources to a collection of Azure SQL Databases

**Requirements**
- It tackles the requirements of unpredictable workloads.
- One may set a maximum level of DTU utilization that may be shared among many databases. 
- If there are larger differences between peak utilization and average utilization per DB
- If the peak utilization for each DB occurs at different points in time


- Azure SQL elastic pool is a cost-effective solution for managing and scaling multiple DBs that have varying and unpredicatable usage demands
- The DBs in an elastic pool are on a single Azure SQL DB serevr and share a fixed number of resources at a fixed price

> Elastic pools can be also created after a set of Azure SQL DBs are provisioned.

## Security layers

Four layers:

1. Network Security
   1. IP firewalls
   2. Virtual network firewall rules: for certain subnerts
2. Access Management
   1. Auth:
      1. SQL auth
      2. Azure AD: groups, identities and roles
   2. Authorization
      1. RBAC
3. Threat Protection
   1. SQL auditing in Azure Monitor and Event Hubs
   2. Advanced: analyzes the logs and checks if there are any potential harms
4. Information Protection
   1. Encryption: always at transport TLS for all connections
   2. Transparent Data Encryption: all raw files are also encrypted. Keys are stored in Azure Key vault
   3. Dynamic Data Masking: senstive data can be masked for non-privileged users

Security Management:
- Vulnerability assesment
- Data discovery and classification

## Scaling

Build on purchasing model and service tier.

Types:
- Vertical: add more compute power. Size of DB
  - CPU Power, Memory, I/O
  - DTU vs vCore models to scale
  - Dynamic Scalability 
  - Any change is almost instant
- Horizontal: add more DBs
  - add more DBs and to shard your data into multiple DB nodes
  - Shards: divided size of a DB in order to distribute the stored data.

Models:
- Single tenancy
  - Each tenant has one DB
- Multi Tenancy
  - Multiple tenants use one DB

## HA and DR options

- Standard availability
  - basic, standard and general purpose service tiers
  - separation of compute and storage
  - performance is degradated during maintenance
- Premium availability
  - Premium or business critical models
  - 

## Backup and Restore

- Full Backup
- Differential Backup
- Transaction log backup