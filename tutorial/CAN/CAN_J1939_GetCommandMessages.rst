CAN J1939 Get Command Messages
*******************************

.. contents:: Contents
    :local:
    

Get Commands 
--------------------------

Get commands are used by other ECUs in the network 
to retrieve information from the OpenIMU300RI.  All Get commands are formed as a 
Request message as specified in SAI J1939-21.  Request Messages are the only 3 byte CAN messages.  
They are used to request PGNs which have a transmission rate of "On Request".  
The format and content of the Request message is **To Be Provided**.  
The following Get Commands are implemented in the example application.  The user can modify the 
existing commands and responses and add commands and responses as needed.


.. toctree::
    :maxdepth: 1
    
.. table::  *Get Commands*
    :align: left

    +------------------------+---------+--------------+
    | **Command**            | **PGN** || **Payload** |
    |                        |         || **Length**  |
    |                        |         || (bytes)     |
    +------------------------+---------+--------------+
    | *Get Firmware Version* | 65242   | 5            | 
    +------------------------+---------+--------------+
    | *Get ECU ID*           | 64965   | 8            | 
    +------------------------+---------+--------------+
    | *Get Hardware BIT*     | 65362   | 2            | 
    +------------------------+---------+--------------+
    | *Get Software BIT*     | 65363   | 2            | 
    +------------------------+---------+--------------+
    | *Get Status*           | 65364   | 2            | 
    +------------------------+---------+--------------+


Responses to Get Commands
--------------------------
The following table describe the payloads for responses to Get Commands


.. table::    *Firmware Version Response Payload*
    :align: left

    +-----------+-----------------------+
    | **Byte**  | **Description**       |
    +-----------+-----------------------+
    | 0         | Major Version Number  |
    +-----------+-----------------------+
    | 1         | Minor Version Number  |
    +-----------+-----------------------+
    | 2         | Patch Number          |
    +-----------+-----------------------+
    | 3         | Stage Number          |
    +-----------+-----------------------+
    | 4         | Build Number          |
    +-----------+-----------------------+

.. table::    *ECU ID Response Payload*
    :align: left

    +----------+---------------------------------+-------------+
    | **Byte** | **Contents**                    | **Acronym** |
    +----------+---------------------------------+-------------+
    | 0        || bit  0   - Data Page           || DP         |
    |          || bit  1   - Reserved Bit 1      || R1         |
    |          || bits 2:4 - Priority            || Prio       |
    |          || bits 5:7 - Reserved Bits 2     || R2         |
    +----------+---------------------------------+-------------+
    | 1        | PDU Format                      | PF          |
    +----------+---------------------------------+-------------+
    | 2        | PDU Specific                    | PS          |
    +----------+---------------------------------+-------------+
    | 3        | Source Address                  | SA          |
    +----------+---------------------------------+-------------+


.. table::    *Hardware BIT Response Payload*
    :align: left

    +---------+-----------------+------------------------------------------+
    | **Bit** | **Description** | **Value**                                |
    +---------+-----------------+------------------------------------------+
    | 0       | MasterFail      | 0=normal, 1=fatal error has occurred     |
    +---------+-----------------+------------------------------------------+
    | 1       | sensorError     | 0=normal, 1=internal hardware error      |
    +---------+-----------------+------------------------------------------+
    | 2       | rateError       | 0=normal, 1=internal gyro error          |
    +---------+-----------------+------------------------------------------+
    | 3       | accelError      | 0=normal, 1=internal accelerometer error |
    +---------+-----------------+------------------------------------------+
    | 4 - 15  | Reserved        | Reserved                                 |
    +---------+-----------------+------------------------------------------+


.. table::    *Software BIT Response Payload*
    :align: left

    +---------+---------------------+-------------------------------------+
    | **Bit** | **Description**     | **Value**                           |
    +---------+---------------------+-------------------------------------+
    | 0       | softwareError       | 0=normal, 1=internal software error |
    +---------+---------------------+-------------------------------------+
    | 1       | algorithmError      | 0=normal, 1=error                   |
    +---------+---------------------+-------------------------------------+
    | 2       | dataError           | 0=normal, 1=error                   |
    +---------+---------------------+-------------------------------------+
    | 3       | initError           | 0=normal, 1=error in algorithm init |
    +---------+---------------------+-------------------------------------+
    | 4       | overRange           | 0=normal, 1=error - reset algorithm |
    +---------+---------------------+-------------------------------------+
    | 5       | CalibrationCRCError | 0=normal, 1=incorrect CRC           |
    +---------+---------------------+-------------------------------------+
    | 6 - 15  | Reserved            | Reserved                            |
    +---------+---------------------+-------------------------------------+


.. table::    *Status Response Payload*
    :align: left

    +---------+-------------------------------+-----------------------------------------------------------------+
    | **Bit** | **Description**               | **Value**                                                       |
    +---------+-------------------------------+-----------------------------------------------------------------+
    | 0       | masterStatus                  | 0=normal, 1=hardware, sensor or software alert                  |
    +---------+-------------------------------+-----------------------------------------------------------------+
    | 1       | hardwareStatus                | 0=normal, 1=programmable alert                                  |
    +---------+-------------------------------+-----------------------------------------------------------------+
    | 2       | softwareStatus                | 0=normal, 1=programmable alert                                  |
    +---------+-------------------------------+-----------------------------------------------------------------+
    | 3       | sensorStatus                  | 0=normal, 1=programmable alert                                  |
    +---------+-------------------------------+-----------------------------------------------------------------+
    | 4       | Reserved                      | Reserved                                                        |
    +---------+-------------------------------+-----------------------------------------------------------------+
    | 5       | Reserved                      | Reserved                                                        |
    +---------+-------------------------------+-----------------------------------------------------------------+
    | 6       | Reserved                      | Reserved                                                        |
    +---------+-------------------------------+-----------------------------------------------------------------+
    | 7       | unlockedEEPROM                | 0=locked, 1=unlocked                                            |
    +---------+-------------------------------+-----------------------------------------------------------------+
    | 8       | algorithmInit                 | 0=normal, 1=in initial mode                                     |
    +---------+-------------------------------+-----------------------------------------------------------------+
    | 9       | Reserved                      | Reserved                                                        |
    +---------+-------------------------------+-----------------------------------------------------------------+
    | 10      | Aceinna_attitude_only_algo    | 0=normal, 1=programmable alert                                  |
    +---------+-------------------------------+-----------------------------------------------------------------+
    | 11      | turnSwitch                    | 0=off, 1=yaw rate greater than turnSwitch threshold             |
    +---------+-------------------------------+-----------------------------------------------------------------+
    | 12      | SensoroverRange               | 0=not asserted, 1=asserted (when Asserted unit should be reset) |
    +---------+-------------------------------+-----------------------------------------------------------------+
    | 13      | Reserved                      | Reserved                                                        |
    +---------+-------------------------------+-----------------------------------------------------------------+
    | 14      | Reserved                      | Reserved                                                        |
    +---------+-------------------------------+-----------------------------------------------------------------+
    | 15      | Reserved                      | Reserved                                                        |
    +---------+-------------------------------+-----------------------------------------------------------------+


.. note:: Use the "CAN J1939 Example Application" Link on the left to follow the flow for the J1939 CAN Application.

