# Azure Data Engineer Exam Preparation
![[DP203-Badge](https://docs.microsoft.com/en-us/learn/certifications/exams/dp-203)](https://img.shields.io/static/v1?label=azure-exam&message=dp203&color=informational) ![[Date-Badge()]](https://img.shields.io/static/v1?label=last-update&message=07.03.22&color=lightgrey)

## Overview of services

  - [Structure vs unstructured data](#structure-vs-unstructured-data)
  - [Data Storage in Azure](#data-storage-in-azure)
  - [Data Lake in Azure](#data-lake-in-azure)
  - [Cosmos DB in Azure](#cosmos-db-in-azure)
  - [Azure SQL Database](#azure-sql-database)
  - [Azure Synapse](#azure-synapse)
  - [Azure Stream Analytics (ASA)](#azure-stream-analytics-asa)
  - [Azure HDInsight](#azure-hdinsight)
  - [Other Services](#other-services)

## Structure vs unstructured data

- Structured Data: data structure is defined at design time. Data structure in form of tables.
  - Microsoft SQL Server
  - Azure SQL Database
  - Azure SQL Data Warehouse
- Nonstructured data: data stored in non-relational system, commonly NoSQL. Data is typically stored in raw format.
  - Key-value store
  - Document databases
  - Graph databases
  - Column databases

## Data Storage in Azure

- Azure Blob: A scalable object store for text and binary data.
- Azure Files: Managed file shares for cloud or on-premises deployments
- Azure Queue: A messaging store for reliable messaging between application components
- Azure Table: A NoSQL store for no-schema storage of structured data

Use cases:

- Cheap storage but no need to query data.
  - Secure, scalable and highly available
  - REST APIs and SDKs available
  - Security: RBAC included
- Data Ingestion: Storage Explorer, AzCopy (up to 1TB), Powershell or Visual Studio.
- For querying, you can use Data Lake Storage Account

## Data Lake in Azure

Azure Data Lake Storage is a Hadoop-compatible data repository that can store any size or type of data. Two versions: Gen1 or Gen2. Gen2 takes advantage of Azure Blob Storage, a hierarchical file system, performance tuning. Users can access data via Blob API or Data Lake File API. Can also be the storage layer for further platforms like **Azure Databricks**, **Hadoop** and **Azure HDInsight**.

Design to store massive amounts of data for big-data analytics.

Key Features:

- Unlimited scalability
- Hadoop compatibility
- Security support for both access control lists (ACLs)
- POSIX compliance
- An optimized Azure Blob File System (ABFS) driver that's designed for big-data analytics
- Zone-redundant storage
- Geo-redundant storage

To ingest data:

- Azure Data Factory
- Apache Sqoop
- Azure Storage Explorer
- AzCopy Tool
- Powershell, VStudio

Query data:

- Gen1: `U-SQL`
- Gen2 Azure Blob API or Azure Data Lake System API

## Cosmos DB in Azure

Globally distributed, multimodel database. You can deploy it by using several API models.

- SQL API
- MongoDB API
- Cassandra API
- Gremlin API
- Table API

Use case: need for a NoSQL database of the supported API model, at a global scale, and with low latency performance. Currently supports a HLA of 99.999% and a response below 10ms when configured correctly.

Data ingestion:

- Azure Data Factory
- Any source application that writes into the DB
- upload JSON docs
- other APIs

To query:

- write stored procedures or UDF
- Use the Javascript API

## Azure SQL Database

Provicdes online transactional processing (OLTP) tha can scale on demand. Profits from scalability and security.

Delivers predictable performance, multiple resource types, service tiers, and compute sizes.

Data Ingestion:

- Developer SDKs
- T-SQL (also for querying or write stored procedures)
- Azure Data Factory

Security:

- Advanced threat protection
- SQL Database auditing
- Data encrpytion
- Azure AD Auth
- Multifactor Auth
- Compliance certification

## Azure Synapse

Use for Data Warehousing and Data Analytics. Key Feature: SQL Pools.

The Data Movement Service (DMS) coordinates and transports data between compute nodes as neccesary. Supports distributed as: hash, round-robin and replicated.

Ingestion with

- SQLBulkCopy
- PolyBase
- Azure Data Factory

Query with T-SQL.

Supports Azure AD auth and high security features.

## Azure Stream Analytics (ASA)

Designed for IoT contexts.

Data Ingestion:

- Azure Event Hubs
- Azure IoT Hub
- Azure Blob Storage

Features:

- Native columnar storage
- Provides support for structured streaming.

## Azure HDInsight

Helps at almost all stages of data processing.

- Hadoop compatible: Apache Hive, HBase, Spark and Kafka.
- Hbase: NoSQL
- Storm: distributed real-time streaming analytics solution
- Kafka: streaming open-source solutions.

## Other Services

- Databricks
- Data Factory: can execute SSIS packages, can take over orchestration and workflows for data movement.
- Data Catalog (metadata store)
