# SQL datastores in Azure summary

## Polybase

- Polybase 6 steps are important
  - Export table to a flat file
  - Create blob storage account
  - Upload flat file to blob storage
  - Run Polybase 6 steps process
    - Create a master key
    - Create a database scoped credential with the storage key
    - Create an external data source
    - Create external file format
    - Create an external table
    - Load from the external table
  - Monitor and confirm succesfull migration
  - Confirm 60 distribution in destination table

## Azure SQL Data Warehouse

- Data distribution is important
  - Three distribution methods:
    - Hash
    - Round-robin
    - Replication

