# No SQL datastores in Azure summary

## Cosmos DB

- 5 APIs are very important:
  - Table (OData and Language Integrated Query - LINQ), SQL, Mongo, Graph (Gremlin)
  - for graph databases, the Gremlin API is the best option
  - for SQL queries, the SQL API is the best option

- Partition keys are important

- Consistency level
    - if the requirement is *most recent commited version* - **strong** consistency is the answer
    - *lowest latency* -> **eventual consistency**

    - Cosmos DB consistency level
        - Strong: This level is guaranteed to return the most recent committed version of a records.
        - Eventual:
          - Lowest latency
          - No guarantee of reading operations using the latest committed write.
        - Session: the same user is guaranteed to read the same value within a single session.
          - Even before replication occurs, the user that writes the data can read the same value.
          - The user at the same location does not mean, they will be in the same session

- Cosmos CLI
  - how to create a Cosmos DB account

Data Lake
  - *hierarhical namespace* in the requirement -> probably cosmos is the right answer
