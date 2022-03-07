# Monitoring Storage and Data Processing in Azure Summary

Important is to understand which metrics are most suitable to which service.

What are the different types of log you will be sending to Log Analytics. 

## How to transfer Databricks logs to Log Analytics/Azure Monitor?

You should use a third-party library to transfer Azure Databricks metrics to Azure Monitor because it is not supported natively at the time of writing.

You should use Azure Log Analytics workspace as the target destination for uploaded Azure Databricks metrics in Azure Monitor. Each workspace has its own data repository, and data sources like Azure Databricks can be configured to store their metrics in a particular Azure Log Analytics workspace.

## Monitoring Azure Databricks

#### Solution for sending application metrics to Azure Monitor?

Dropwizard is a Java library. Spark, which is the cluster engine that is used to run Databricks, uses a configurable metrics system that is based on the Dropwizard Metrics Library.


### Diagnostic logs

You should configure diagnostics logs to send data to a blob storage account.

By default, Azure Data Pipeline stores run-data for only 45 days. To store the data longer than that, you must configure diagnostics logs. With diagnostics logs, you can choose to store the run-data in a blob storage account, an event hub, or a Log Analytics workspace.

You can query using KQL in the Log Analytics workspace tables.

ADFPipelineRun table contains rows for status changes like InProgress and Succeeded.

AzureMetrics contains metrics like PipelineSucceededRuns


### Azure Data Factory inbuilt monitoring

Azure Data Factory includes monitoring capabilities for your pipeline runs with execution metrics and pipeline status. You can define alerts directly in Azure Data Factory Monitor.

However, Azure Data Factory data retention is limited to 45 days. You need to use Azure Monitor for longer retention.


### Azure Stream analytics – Metrics to monitor

#### SU monitoring

You should use the SU % utilization metric. This metric indicates how much of the provisioned SU % is in use. If this indicator reaches 80%, there is a high probability that the job will stop.

#### Input events metric - This metric counts the number of records deserialized from the input events.

#### Runtime errors metric - This metric is the total number of errors during query processing.

Understand Stream Analytics job monitoring and how to monitor queries


### Azure Network Watcher

It is a centralized tool for monitoring Azure networking.