# Batch processing in Azure summary

## Azure Data Factory

- Integration runtime is an important topic. Different runtimes:
  - SSIS
    - To use existing SSIS packages
  - Azure:
    - This is required when you need to copy data between Azure and public cloud services
  - self-hosted:
    - When you use copy activity to copy data from Azure and a private network, you must use the self-hosted integration runtime

## Azure Databricks

This is an Apache Spark-based technology that allows you to run code in notebooks.

Code can be written in SQL, Python, Scala, and R.

You can have data automatically generate pie charts and bar charts when you run a notebook.

You can override the default language by specifying the language magic command `<language>` at the beginning of a cell.