---
title: "Guidelines for Accessing Attribute Values"
permalink: /guides/attribute-access-guidelines/
group: versions
layout: post
color: green
icon: fa fa-file-alt
---

<!-- markdownlint-disable no-duplicate-heading -->

The following sections contain guidelines for accessing attribute values efficiently in the Metasys API.

## Guidelines for batch reading of attribute values

Reading attribute values using batch operations, that is, polling points, can affect engine and server performance and the response times for queries. To ensure optimal performance and response times, use the following guidelines for batch reading of attribute values:

- The supported maximum for polling is 750 points per engine.
- The supported maximum for streaming is 1,500 points per engine.
- The supported server maximums are as follows:
- Use polling for points that change their value frequently, for example, flow, pressure, and output values. For points that do not change their value frequently, for example, zone air temperature and heating output, use streaming.
- Do not poll points more frequently than once per minute.
- To reduce the number of points polled, use the COV Min Time attribute at the point and at the engine.
- Run batch operations only for as long as you need the data.
- Always close the connection after you receive the response.

## Detailed example for polling and streaming

The following example illustrates how to use polling and streaming while still maintaining the necessary resource performance to properly run Metasys software.

**Note**: All values in the following tables are approximate values.

The following table shows an example SNE with the number of points that generally need polling and streaming for an average monitoring scenario:

| Systems           | Points/system | Total points | Polling | Streaming |
| ----------------- | ------------: | -----------: | ------: | --------: |
| 72 VAVs           |            24 |         1728 |     288 |       288 |
| 2 AHUs            |            44 |           88 |      20 |        36 |
| 7 UH              |            22 |          154 |       7 |        77 |
| 3 Exh fans        |             6 |           18 |       0 |         9 |
| 1 Sump pump       |             2 |            2 |       0 |         1 |
| Total: 85         |            98 |         1990 |     315 |       411 |

For a heavy monitoring scenario, see the following table:

| Systems           | Points/system | Total points | Polling | Streaming |
| ----------------- | ------------: | -----------: | ------: | --------: |
| 72 VAVs           |            24 |         1728 |     288 |       792 |
| 2 AHUs            |            44 |           88 |      18 |        50 |
| 7 UH              |            22 |          154 |      14 |        98 |
| 3 Exh fans        |             6 |           18 |       0 |        18 |
| 1 Sump pump       |             2 |            2 |       0 |         2 |
| Total: 85         |            98 |         1990 |     320 |       960 |

### VAV example

The following table contains examples of VAV points for polling and streaming in an average monitoring scenario. The points in the table represent only a subset of the typical points in an engine.

| Point name | Long name                      | Polling | Streaming |
| ---------- | ------------------------------ | :-----: | :-------: |
| AUTOCAL-C  | Autocalibration Command        |         |           |
| FLUSHPOS | Water Flush Valve Position       |         |           |
| HTG-EN | Heating Enable                     |         |           |
| OCC-SCHEDULE | Occupancy Schedule           |         |           |
| EFF-OCC | Effective Occupancy               |         |     x     |
| SA-T | Supply Air Temperature               |         |           |
| SYSTEM-MODE | System Mode                   |         |           |
| TUNING-RESET | Tuning Reset                 |         |           |
| UNITEN-MODE | Unit Enable Mode              |         |           |
| WC-C | Warmup Cooldown Command              |         |           |
| WC-S | Warmup Cooldown Status               |         |           |
| ZNT-STATE | Zone Temperature State          |         |           |
| ZN-T | Zone Air Temperature                 |         |     x     |
| ZN-SP | Zone Setpoint                       |         |           |
| EFFCLG-SP | Effective Cooling Setpoint      |         |     x     |
| EFFHTG-SP | Effective Heating Setpoint      |         |     x     |
| CLG-MAXFLOW | Cooling Maximum Flow Setpoint |         |           |
| HTG-MINFLOW | Heating Minimum Flow Setpoint |         |           |
| DA-T | Discharge Air Temperature            |         |           |
| SA-F* | Supply Air Flow                     |    x    |           |
| DA-VP | Discharge Air Velocity Pressure     |         |           |
| SAFLOW-SP* | Supply Air Flow Setpoint       |    x    |           |
| DPR-O* | Damper Output                      |    x    |           |
| HTG-O* | Heating Output                     |    x    |           |
|        |                                 24 |       4 |         4 |

**Notes**:

- SA-F: Changes frequently and cannot be controlled through the API. Only poll in the frequency that you need, but normally it is not needed every minute.
- If the polled points use COV Min Time (by default 15 seconds), then you can move to streaming.

### AHU example

The following table contains examples of AHU points for polling and streaming in an average monitoring scenario. The points in the table represent only a subset of the typical points in an engine.

| Point name   | Long name                          | Polling | Streaming |
| ------------ | ---------------------------------- | :-----: | :-------: |
| DA1-P        | Discharge Air Pressure 1           | x       |           |
| DA-T         | Discharge Air Temperature          |         | x         |
| BDLG-SP      | Building Pressure Setpoint         | x       |           |
| CLG-EN       | Cooling Enable                     |         |           |
| CLGUNOCC-SP  | Unoccupied Cooling Setpoint        |         |           |
| DAP-SP       | Discharge Air Pressure Setpoint    |         | x         |
| DAT-SP       | Discharge Air Temperature Setpoint |         | x         |
| HTGUNOCC-SP  | Unoccupied Heating Setpoint        |         |           |
| HUM-EN       | Humidifier Enable                  |         |           |
| HUM-SP       | Humidifier Setpoint                |         | x         |
| MOAFLOW-SP   | Minimum Outdoor Air Flow Setpoint  |         | x         |
| OA-T\*       | Outdoor Air Temperature            |         |           |
| OCC-OVERRIDE | Occupancy Override                 |         |           |
| OCC-SCHEDULE | Occupancy Schedule                 |         |           |
| EFF-OCC      | Effective Occupancy                |         | x         |
| PH-EN        | Preheat Enable                     |         |           |
| RH-EN        | Reheat Enable                      |         |           |
| TUNING-RESET | Tuning Reset                       |         |           |
| UNITEN-MODE  | Unit Enable Mode                   |         | x         |
| WC-C         | Warmup Cooldown Command            |         |           |
| BLDG-P\*     | Building Pressure                  | x       |           |
| DAPHI-A      | Discharge Air Pressure High Alarm  |         | x         |
| LT-A         | Low Temperature Alarm              |         | x         |
| MA-T         | Mixed Air Temperature              |         | x         |
| OA-F\*       | Outdoor Air Flow                   | x       |           |
| PFILT-S      | Pre-filter Status                  |         |           |
| PH-T         | Preheat Air Temperature            |         | x         |
| RA-H         | Return Air Humidity                |         | x         |
| RA-T         | Return Air Temperature             |         | x         |
| RF-S         | Return Fan Status                  |         | x         |
| SF-S         | Supply Fan Status                  |         | x         |
| CLG-O\*      | Cooling Output                     | x       |           |
| HUM-C        | Humidifier Command                 |         | x         |
| HUM-O\*      | Humidifier Output                  | x       |           |
| MAD-O\*      | Mixed Air Damper Output            | x       |           |
| PH-O\*       | Preheat Output                     | x       |           |
| RF-C         | Return Fan Command                 |         | x         |
| RF-O\*       | Return Fan Output                  | x       |           |
| SF-C         | Supply Fan Command                 |         | x         |
| SF-O\*       | Supply Fan Output                  | x       |           |
| ECONSWO-SP   | Economizer Switchover Setpoint     |         |           |
| OALT-SP      | Low OA Temperature Setpoint        |         |           |
| LT-SP        | Low Temperature Setpoint           |         |           |
| AHU-STATE    | Air Handling Unit State            |         |           |
|              | 44                                 | 10      | 18        |

**Notes**:

- OAT-T: If points are shared, sign up only for data from the master (sensor) object.
- If the polled points use COV Min Time (by default 15 seconds), then you can move to streaming.

### UH example

The following table contains examples of UH points for polling and streaming in an average monitoring scenario. The points in the table represent only a subset of the typical points in an engine.

| Point name   | Long name                             | Polling | Streaming |
| ------------ | ------------------------------------- | :-----: | :-------: |
| DATLL-SP     | Discharge Air Temp Low Limit Setpoint |         |           |
| HTG-EN       | Heating Enable                        |         |           |
| OA-T\*       | Outdoor Air Temperature               |         |           |
| OCC-SCHEDULE | Occupancy Schedule                    |         |           |
| TUNING-RESET | Tuning Reset                          |         |           |
| UNITEN-MODE  | Unit Enable Mode                      |         | x         |
| DA-T         | Discharge Air Temperature             |         | x         |
| FILT-S       | Filter Status                         |         |           |
| LT-A         | Low Temperature Alarm                 |         | x         |
| ZN-SP        | Zone Setpoint                         |         |           |
| ZN-T         | Zone Air Temperature                  |         | x         |
| ZN-TOCC      | Zone Temporary Occupancy              |         |           |
| ZNF-O\*      | Zone Fan Output                       | x       |           |
| HTG1-C       | Heating Stage 1 Command               |         | x         |
| HTG2-C       | Heating Stage 2 Command               |         | x         |
| HTG3-C       | Heating Stage 3 Command               |         | x         |
| SFH-C        | Supply Fan High Command               |         | x         |
| SFL-C        | Supply Fan Low Command                |         | x         |
| SFM-C        | Supply Fan Medium Command             |         | x         |
| EFFHTG-SP    | Effective Heating Setpoint            |         | x         |
| OALL-SP      | Outdoor Air Low Limit Setpoint        |         |           |
| HTG-O        | Heating Output                        |         |           |
|              | 22                                    | 1       | 11        |

**Notes**:

- OAT-T: If points are shared, sign up only for data from the master (sensor) object.
- If the polled points use COV Min Time (by default 15 seconds), then you can move to streaming.

### EF example

The following table contains examples of EF points for polling and streaming in an average monitoring scenario. The points in the table represent only a subset of the typical points in an engine.

| Point name   | Long name                          | Polling | Streaming |
| ------------ | ---------------------------------- | :-----: | :-------: |
| EF1-C        | Exhaust Fan 1 Command              |         |           |
| EF1-S        | Exhaust Fan 1 Status               |         |     x     |
| EF2-C        | Exhaust Fan 2 Command              |         |           |
| EF2-S        | Exhaust Fan 2 Status               |         |     x     |
| EF3-C        | Exhaust Fan 3 Command              |         |           |
| EF3-S        | Exhaust Fan 3 Status               |         |     x     |
|              | 6                                  |    0    |     3     |

### Sump pump example

The following table contains examples of sump pump points for polling and streaming in an average monitoring scenario.

| Point name   | Long name                          | Polling | Streaming |
| ------------ | ---------------------------------- | :-----: | :-------: |
| SP1-C        | Sump Pump 1 Command                |         |     x     |
| SP1-S        | Sump Pump 1 Status                 |         |           |
