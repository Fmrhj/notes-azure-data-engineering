# Batch processing services in Azure

## Data Factory

Cloud based ETL/ELT orchestration tool.

Where to use it:

- Migration: Azure provides more specific services for that. It suits better to periodic data loads and transformation jobs.
- Streaming: for jobs more frequent than 5 minutes, Streaming alternatives shall be used.
- Transformations: if simple yes, more complicated logic Databricks or Hbase.

### Components

- Pipeline
- Linked Service:
  - represents a connection information (including credentials) to connect to external sources
- Sources: Datasets
- Sinks: Datasets
- Activities: processes in the pipeline. Actions to perform on data.
  - Data movement
  - Data transformation: to transform and enrich data
  - Control activities (control pipeline constructs, e.g. ForeEach, if)
- Data Flow
- Integration Runtime (IR)
  - Fully managed serverless compute infrastructure:
  - Azure, self-hosted or Azure-SSIS
- Triggers
  - Event that initiates a pipeline execution
  - Schedule
  - Tumbuling Window Trigger: operates on a periodic interval, also retain state
    - One-to-one relationship
    - Advance configuration options
    - Establishes relationships with other triggers based on offset (+/- an amount of hours) and size to the time windows (e.g. hours) 
  - Event-based:
    - e.g. on Blob Storage 
    - Event grid service
  - Data Flow
    - Uses a UI to build pipelines
    - It provides a menu of built-in transformations to perform on data
    - Data flow can abstract a complete logic into one activity


## Azure Databricks

Databricks on Azure. It provides the processing capabilities of Spark and embedd this into Azure.

### Cluster Types

- Interactive
  - Multiple users interactively analyze the data together
  - Low execution time
  - Auto scale on demand
  - Manually terminate
- Job Cluster
  - Created and terminated for running automated data
  - Terminates when job ends
  - Auto scale on demand
  - cheaper

### Cluster modes

- Standard
  - Single user
  - no fault isolation
  - no task preemption
  - supports Scala, Python SQL, R & Java
- High concurrency mode
  - Multiple users
  - fault isolation
  - task preemption
  - maximum cluster utilization
  - Only supports Python, SQL & R 

### Components

- Workspace:
  - Isolation Unit
  - Workspace ID
  - Locked Resources
  - Organize Assets
  - Access Control
- Notebooks
  - Languages
  - Workflows
  - Execution
  - Visualization
  - Collaboration
- Jobs
- Libraries
- Database and Tables