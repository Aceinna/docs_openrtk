CAN J1939 Data Packet Messages
*******************************

.. contents:: Contents
    :local:
    
.. toctree::
    :maxdepth: 1
    

Data Packet Types
-----------------

The following Data Packet messages are implemented in the example application.
The user can modify the provided messages or add messages as needed.

The OpenIMU300RI output 3 types of data packets: slope sensor information - type 2 (SSI2), 
angular rate sensor data, and accelerometer sensor data.  The data packets are sent at accelerometerrate 
that can be set by other ECUs.

.. table:: *Data Packet Messages*
    :align: left

    +-------------------+-----------+--------------+--------------------------------+
    | **Data Packet**   | **PGN**   || **Payload** || **Purpose*                    |
    ||                  |           || **Length**  ||                               |
    ||                  |           || (bytes)     ||                               |
    +-------------------+-----------+--------------+--------------------------------+
    || Slope Sensor     | 61481     || 8           || Provide high accuracy         |
    || Information      |           ||             || pitch & roll rates            |
    || Type 2           |           ||             ||                               | 
    +-------------------+-----------+--------------+--------------------------------+
    || Angular Rate     | 61482     || 8           || Provide moderate accuracy     |
    || Sensor Data      |           ||             || pitch, roll and yaw rates     |
    +-------------------+-----------+--------------+--------------------------------+
    || Acceleration     | 61485     || 8           || Provide moderate accuracy     |
    || Sensor Data      |           ||             || X, Y, and Z axes acceleration |
    +-------------------+-----------+--------------+--------------------------------+


*Slope Sensor Information - Type 2 (SSI2) Data Packet*
------------------------------------------------------

The following table describes the SSI2 Data Packet payload:

.. table:: *SSI2 Data Packet Payload*
    :align: left

    +----------------+------------+------------------+-----------------+------------+
    | **Field Name** | **Length** | **Range**        | **Resolution**  | **Offset** |
    +----------------+------------+------------------+-----------------+------------+
    | Pitch          | 24 bits    | -250 to +252 deg | 1/32768 deg/bit | -250 deg   |
    +----------------+------------+------------------+-----------------+------------+
    | Roll           | 24 bits    | -250 to +252 deg | 1/32768 deg/bit | -250 deg   |
    +----------------+------------+------------------+-----------------+------------+
    | FoM,Latency    | 16 bits    | Ignore           | Ignore          | Ignore     |
    +----------------+------------+------------------+-----------------+------------+

*Angular Rate Data Packet*
---------------------------------

The following table describes the Angular Rate Data Packet payload:

.. table:: *Angular Rate Data Packet Payload*
    :align: left

    +----------------+------------+--------------------+----------------------+------------+
    | **Field Name** | **Length** | **Range**          | **Resolution**       | **Offset** |
    +----------------+------------+--------------------+----------------------+------------+
    | Pitch (X)      | 2 bytes    | -250 to +252 deg/s | 1/256 deg/second/bit | -250 deg   |
    +----------------+------------+--------------------+----------------------+------------+
    | Roll  (Y)      | 2 bytes    | -250 to +252 deg/s | 1/256 deg/second/bit | -250 deg   |
    +----------------+------------+--------------------+----------------------+------------+
    | Yaw (Z)        | 2 bytes    | -250 to +252 deg/s | 1/256 deg/second/bit | -250 deg   |
    +----------------+------------+--------------------+----------------------+------------+
    | FoM,Latency    | 2 bytes    | Ignore             | Ignore               | Ignore     |
    +----------------+------------+--------------------+----------------------+------------+

*Acceleration Data Packet*
-----------------------------------

The following table describes the Acceleration Data Packet payload:

.. table:: *Acceleration Data Packet Payload*
    :align: left

    +--------------------+------------+-----------------------+-----------------+-------------+
    || **Accelerometer** | **Length** | **Range**             | **Resolution**  | **Offset**  |
    || **Field Name**    |            |                       |                 |             |
    +--------------------+------------+-----------------------+-----------------+-------------+
    | X                  | 2 bytes    | -320 to 320/55 m/s**2 | 0.01 m/s**2/bit | -320 m/s**2 |
    +--------------------+------------+-----------------------+-----------------+-------------+
    | Y                  | 2 bytes    | -320 to 320/55 m/s**2 | 0.01 m/s**2/bit | -320 m/s**2 |
    +--------------------+------------+-----------------------+-----------------+-------------+
    | Z                  | 2 bytes    | -320 to 320/55 m/s**2 | 0.01 m/s**2/bit | -320 m/s**2 |
    +--------------------+------------+-----------------------+-----------------+-------------+
    | FoM,Latency        | 2 bytes    | Ignore                | Ignore          | Ignore      |
    +--------------------+------------+-----------------------+-----------------+-------------+


