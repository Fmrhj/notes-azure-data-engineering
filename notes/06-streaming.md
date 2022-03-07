# Data streaming services in Azure

## Azure Stream Analytics Service

### Overview

Azure Stream Analytics (ASA) Service is a fully managed, real-time analytics service designed to process fast moving streams of data.

### Dataflow

- Ingest_
  - From:
    - IoT devices
    - Log, Files
    - Customer data, Financial transactions
    - Weather data
    - Business App
  - Into Azure Services:
    - **Event Hubs**
    - **Azure Blob Storage**
    - **IoT Hub**
- Analyze
  - Continuous Intelligence / Real-time analytics
  - Reference Data
  - Real Time scoring
- Deliver
  - Alerts or actions
    - Event Hub
    - Service Bus
    - Azure Functions
  - Dynamisch Dashboarding (Power BI)
  - Data Warehousing (Synapse)
  - Storage / Archival
    - SQL DB
    - Azure Data Lake Gen1/2
    - Cosmos DB
    - Blob Storages


### Windowing Functions

- Each data event has a timestamp
- An operation is performed on the events of a certain time window
- Four types:
  - Tumbling
    - Divide the time into equally non-overlapping buckets 
    - e.g. how many events every x seconds/unit of time
    - does not overlapp
  - Hopping
    - The time windows are overlapping
    - e.g. every *y* seconds give me count of events over the last *x* seconds
    - above: *x* is the length of the hopping window and *y* is the length of the *hop*
    - it overlapps
  - Sliding
    - Window has a fixed length (function parameter)
    - After an event is recorded, go back *x* units of time, where *x* is the length of the sliding window
    - it overlapps
  - Session
    - The length is not fixed (only window without a fixed length)
    - This window does not overlapps
    - The window starts after an event and keeps listening events. If there are no events for a period of time, it shuts down. There is a parameter which controls the maximum length of a window
  

```sql
/*  Tumbling Window */
SELECT timezone, COUNT(*) AS count
FROM strean TIMESTAMP BY created_at
GROUP BY timezone, TumblingWindow(second, 10)

/* Hoping  Window */
SELECT topic, COUNT(*)
FROM stream TIMESTAMP BY created_at
GROUP BY topic, HopingWindow(minute, 10, 5)

/*  Sliding Window */
SELECT topic, COUNT(*)
FROM stream TIMESTAMP BY created_at
GROUP BY topic, SlidingWindow(minute, 10)

/*  Session Window */
SELECT topic, COUNT(*)
FROM stream TIMESTAMP BY created_at
GROUP BY topic, SessionWindow(minute, 5, 10)
 ```

 ### Strem Anayltics Jobs

 It runs a logic (query) as a streaming job. 

 **Job Topology**
 - where we define inputs and outpus
 - Input
   - Event Hub
   - IoT Hub
   - Blob storage
 - Output
   - min. row configuration
   - max. time
   - 

**Configure**
 - add storage accounts
 - scale up
 - error policy

## IoT Hub

Streaming device to process data from many (in order of million) devices. This source can push data into streaming analytics jobs.

Example

```sql
SELECT *
FROM strmoutputblob
FROM strminputblob
HAVING temperature > 27
```

> A Microsoft supported [Raspberry Py IoT](https://azure-samples.github.io/raspberry-pi-web-simulator/) simulator