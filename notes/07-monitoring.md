# Monitoring Azure Data Services

## Monitoring Data Storage

### Azure Monitor Service 

Sources
- Application
- Operating System
- Azure Resources
- Azure Subscription
- Azure Tenant
- Custom Sources

Metrics and Logs to:
- Insights
  - Application
  - Container
  - VVM
  - Monitoring Solutions
- Visualize
  - Dashboards
  - Views
  - Power BI
  - Workbooks
- Analyze
  - Metric Analysi
  - Log Analytics
- Respond
  - Alerts
  - Autoscale
- Integrate
  - Logic Apps
  - Export APIs

### Monitoring Logs

- Create alerts using the DSL to write queries
- Workbooks
  - unified experience for services
  - there are many ready-to-go workbooks to use
- Insights
- Settings

## Monitoring Storage

- Blob and Data Lake have the same monitoring tools
- Options
  - Insights/Workbooks
  - Metrics/Alerts
  - Classic Diagnostic settings
    - Logging aspects and retention period

## Monitoring Synapse

- Monitoring query level activity
- Set an alert
- Examine metrics
- Diagnostic settings

## Monitoring Cosmos

- Very little to monitor Cosmos

## Monitoring Data Factory

- Use the monitor tab in ADF or Synapse
- You can set metrics on different levels, e.g. pipeline level, for ADF

## Monitoring Databricks

- Use ganglia
- Show snapshots of live data
- Azure Monitor
  - No native support for databricks
  - Dropwizard metrics library (log4j)
- Use grafana

## Monitoring ASA

- Jobs can be monitored
  - use different SDKs
- Important metrics
  - SU % Utilization
  - RunTime Error
  - Watermark dely
  - Input deserialization error
  - Backlogged Input Events
  - Data Conversion Errors