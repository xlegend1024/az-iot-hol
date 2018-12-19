# Microsoft Azure IoT Hands-on Lab

## Architecture

![arch](./images/az-iot-lab-archi.png)

## Setup Hands-on Lab Environment

## [00. Create lab environment](https://github.com/xlegend1024/az-iot-hol/blob/master/00CreateLab.md)

> __Login Windows Server Virtual Machine for rest of Labs__
> Login Windows Server VM
> Login Azure inside of the VM

Login Windows Server VM, "**azlab###wvm**" and __Download__ the sample project from https://github.com/Azure-Samples/azure-iot-samples-node/archive/master.zip.

## 01. Create Cloud Gateway

### Lab 1. Create Azure IoT Hub

### [Lab 2. Device-to-Cloud (D2C)](https://docs.microsoft.com/en-us/azure/iot-hub/quickstart-send-telemetry-node)

## 02. Time Serise Insight

### Lab 3. Create Time Serise Insight and Visualize data

### [Lab 4. Cloud-to-Device (C2D)](https://docs.microsoft.com/en-us/azure/iot-hub/quickstart-control-device-node)

## 03. IoT Edge

### [Lab 5. Deploy IoT Edge](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux)

## 04. Real-time Analytics

### Lab 6. Create Azure Stream Analytics Jobs

Store raw data

* Use CosmosDB and Blob
* Store recored by recored

> To save logs in Blob, use following sample 

```
{datetime:yyyy}/{datetime:MM}/{datetime:dd}/{datetime:HH}/{datetime:mm}
```

Store aggregated data

* Use SQL Database
* 1 Minute and 1 Hour aggregation

[Stream Analytics Query](https://raw.githubusercontent.com/xlegend1024/az-iot-hol/master/StreamAnalyticJobs/query.sql)

```sql
SELECT
    *
INTO
    cosmosdb
FROM
    iothub

SELECT
    input.IoTHub.ConnectionDeviceId as deviceID, timeCreated, temperature as machine_temp, pressure as machine_press, temperature as ambient_temp, humidity as ambient_humi
INTO
    blob
FROM
    iothub as input

SELECT
    input.IoTHub.ConnectionDeviceId as deviceID, System.TimeStamp AS timeCreated, AVG(temperature) as machine_temp, AVG(pressure) as machine_press, avg(temperature) as ambient_temp, avg(humidity) as ambient_humi
INTO
    sqldb
FROM
    iothub as input
GROUP BY
    input.IoTHub.ConnectionDeviceId, TumblingWindow(minute, 1)

SELECT
    input.IoTHub.ConnectionDeviceId as deviceID, System.TimeStamp AS timeCreated, AVG(temperature) as machine_temp, AVG(pressure) as machine_press, avg(temperature) as ambient_temp, avg(humidity) as ambient_humi
INTO
    sqldb2
FROM
    iothub as input
GROUP BY
    input.IoTHub.ConnectionDeviceId, TumblingWindow(hour, 1)
```
