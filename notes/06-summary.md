# Streaming processing in Azure summary

## ASA

**Window functions are important**
- Tumbling Window
  - non-overlapping
- Hoping Window
- Sliding Window
- Session Window

| Tumbling Window                 | Hoping window                                                   | Sliding window                                       | Session window                     |
|---------------------------------|-----------------------------------------------------------------|------------------------------------------------------|------------------------------------|
| Fixed size                      | Fixed size                                                      | Fixed size                                           | Varible size                       |
| Non-overlapping                 | Overlap                                                         | Overlap                                              | non-overlapping                    |
| Each event is only counted once | Hop forward, hence an event <br>could be counted more than once | Sliding windows may contain <br>events from the past | Each event only counted once       |
| Parameters: (window length)     | Parameters: (hop length, window length)                         | Parameters: (window length)                          | Parameters: (timeout, max. length) |

**Difference between Event Hub and IoT Hub**

| Event Hub                                                                                                                                                               | IoT Hub                                                                                                                                            |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| Event Hub is an Azure resource that allows you<br> to stream big data to the cloud.                                                                                     | IoT Hub is an Azure resource that allows you to <br>stream big data to the cloud.                                                                  |
| Event Hub accepts streaming telemetry data <br>from other sources. It is basically a big data pipeline. <br>It allows you to capture, retain, and replay telemetry data | It supports per-device provisioning.                                                                                                               |
| It accepts streaming data over HTTPS and AMQP.                                                                                                                          | It accepts streaming data over HTTPS, AMQP, <br>and Message Queue Telemetry Transport (MQTT).                                                      |
| A Stream Analytics job can read data from Event Hubs <br>and store the transformed data in a variety of output data sources, <br>including Power BI.                    | A Stream Analytics job can read data from IOT Hubs <br>and store the transformed data in a variety of output data sources, <br>including Power BI. |