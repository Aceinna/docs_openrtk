DMUx81 Standard UART Port Commands and Messages
***********************************************

Link Test
---------

Ping Command
------------

+----------------------+-------------+--------+-------------+
| Ping (‘PK’ = 0x504B) |             |        |             |
+----------------------+-------------+--------+-------------+
| Preamble             | Packet Type | Length | Termination |
+----------------------+-------------+--------+-------------+
| 0x5555               | 0x504B      | -      | -           |
+----------------------+-------------+--------+-------------+

The ping command has no payload. Sending the ping command will cause the
unit to send a ping response. To facilitate human input from a terminal,
the length and CRC fields are not required. (Example: 0x5555504B009ef4
or 0x5555504B))

Ping Response
-------------

+----------------------+-------------+--------+-------------+
| Ping (‘PK’ = 0x504B) |             |        |             |
+----------------------+-------------+--------+-------------+
| Preamble             | Packet Type | Length | Termination |
+----------------------+-------------+--------+-------------+
| 0x5555               | 0x504B      | 0x00   | <CRC (U2)>  |
+----------------------+-------------+--------+-------------+

The unit will send this packet in response to a ping command.

Echo Command
------------

+----------------------+-------------+--------+----------------+-------------+
| Echo (‘CH’ = 0x4348) |             |        |                |             |
+----------------------+-------------+--------+----------------+-------------+
| Preamble             | Packet Type | Length | Payload        | Termination |
+----------------------+-------------+--------+----------------+-------------+
| 0x5555               | 0x4348      | N      | <echo payload> | <CRC (U2)>  |
+----------------------+-------------+--------+----------------+-------------+

The echo command allows testing and verification of the communication
link. The unit will respond with an echo response containing the *echo
data*. The *echo data* is N bytes long.

Echo Response
-------------

+-----------+-----------+-----------+-----------+-----------+-----------+
| Echo      |           |           |           |           |           |
| Payload   |           |           |           |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| Byte      | Name      | Format    | Scaling   | Units     | Descripti |
| Offset    |           |           |           |           | on        |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 0         | echoData0 | U1        | -         | -         | first     |
|           |           |           |           |           | byte of   |
|           |           |           |           |           | echo data |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 1         | echoData1 | U1        | -         | -         | Second    |
|           |           |           |           |           | byte of   |
|           |           |           |           |           | echo data |
+-----------+-----------+-----------+-----------+-----------+-----------+
| …         | …         | U1        | -         | -         | Echo data |
+-----------+-----------+-----------+-----------+-----------+-----------+
| N-2       | echoData. | U1        | -         | -         | Second to |
|           | ..        |           |           |           | last byte |
|           |           |           |           |           | of echo   |
|           |           |           |           |           | data      |
+-----------+-----------+-----------+-----------+-----------+-----------+
| N-1       | echoData… | U1        | -         | -         | Last byte |
|           |           |           |           |           | of echo   |
|           |           |           |           |           | data      |
+-----------+-----------+-----------+-----------+-----------+-----------+

Interactive Commands
--------------------

Interactive commands are used to interactively request data from the
DMUx81ZA Series, and to calibrate or reset the DMUx81ZA Series.

Get Packet Request
------------------

+----------------------------+-------------+--------+--------------+-------------+
| Get Packet (‘GP’ = 0x4750) |             |        |              |             |
+----------------------------+-------------+--------+--------------+-------------+
| Preamble                   | Packet Type | Length | Payload      | Termination |
+----------------------------+-------------+--------+--------------+-------------+
| 0x5555                     | 0x4750      | 0x02   | <GP payload> | <CRC (U2)>  |
+----------------------------+-------------+--------+--------------+-------------+

This command allows the user to poll for both measurement packets and
special purpose output packets including ‘T0’, ‘VR’, and ‘ID’.

+-----------+-----------+-----------+-----------+-----------+-----------+
| GP        |           |           |           |           |           |
| Payload   |           |           |           |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| Byte      | Name      | Format    | Scaling   | Units     | Descripti |
| Offset    |           |           |           |           | on        |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 0         | requested | U2        | -         | -         || The      |
|           | PacketType|           |           |           | requested |
|           |           |           |           |           || packet   |
|           |           |           |           |           | type      |
+-----------+-----------+-----------+-----------+-----------+-----------+

Refer to the sections below for Packet Definitions sent in response to
the ‘GP’ command

Algorithm Reset Command
-----------------------

+---------------------------------+-------------+--------+---------+-------------+
| Algorithm Reset (‘AR’ = 0x4152) |             |        |         |             |
+---------------------------------+-------------+--------+---------+-------------+
| Preamble                        | Packet Type | Length | Payload | Termination |
+---------------------------------+-------------+--------+---------+-------------+
| 0x5555                          | 0x4152      | 0x00   | -       | <CRC (U2)>  |
+---------------------------------+-------------+--------+---------+-------------+

This command resets the state estimation algorithm without reloading
fields from EEPROM. All current field values will remain in affect. The
unit will respond with an algorithm reset response.

Algorithm Reset Response
------------------------

+---------------------------------+-------------+--------+-------------+
| Algorithm Reset (‘AR’ = 0x4152) |             |        |             |
+---------------------------------+-------------+--------+-------------+
| Preamble                        | Packet Type | Length | Termination |
+---------------------------------+-------------+--------+-------------+
| 0x5555                          | 0x4152      | 0x00   | <CRC (U2)>  |
+---------------------------------+-------------+--------+-------------+

The unit will send this packet in response to an algorithm reset
command.

Calibrate Command
-----------------

+---------------------------+-------------+--------+--------------+-------------+
| Calibrate (‘WC’ = 0x5743) |             |        |              |             |
+---------------------------+-------------+--------+--------------+-------------+
| Preamble                  | Packet Type | Length | Payload      | Termination |
+---------------------------+-------------+--------+--------------+-------------+
| 0x5555                    | 0x5743      | 0x02   | <WC payload> | <CRC (U2)>  |
+---------------------------+-------------+--------+--------------+-------------+

This command allows the user to perform various calibration tasks with
the DMUx81ZA Series. See the calibration command table below for
details. The unit will respond immediately with a calibrate response
containing the *calibrationRequest* received or an error response if the
command cannot be performed.

+-----------+-----------+-----------+-----------+-----------+-----------+
| WC        |           |           |           |           |           |
| Payload   |           |           |           |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| Byte      | Name      | Format    | Scaling   | Units     | Descripti |
| Offset    |           |           |           |           | on        |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 0         || calib-   | U2        | -         | -         || The      |
|           || Request  |           |           |           | requested |
|           |           |           |           |           || calibrati|
|           |           |           |           |           | on        |
|           |           |           |           |           | task      |
+-----------+-----------+-----------+-----------+-----------+-----------+

Currently, magnetic alignment is the only function supported by the
calibrate command. There are two magnetic alignment procedures
supported; (1) magnetic alignment with automatic yaw tracking
termination, and magnetic alignment without automatic termination.

+-----------------------------------+-----------------------------------+
| **calibrationRequest**            | **Description**                   |
+-----------------------------------+-----------------------------------+
| 0x0009                            || Begin magnetic alignment         |
|                                   | without automatic termination.    |
|                                   || Rotate vehicle through >360      |
|                                   | degrees yaw and then send 0x000B  |
|                                   || calibration request to terminate.|
+-----------------------------------+-----------------------------------+
| 0x000B                            || Terminate magnetic alignment. The|
|                                   | unit will send a CC response      |
|                                   || containing the hard-iron and     |
|                                   | soft-iron values. To accept the   |
|                                   || parameters, store them using the |
|                                   | write magnetic calibration        |
|                                   || command.                         |
+-----------------------------------+-----------------------------------+
| 0x000C                            || Begin magnetic calibration with  |
|                                   | automatic termination. Rotate the |
|                                   || unit through x81 degrees in yaw. |
|                                   | The unit will send a CC response  |
|                                   || containing the hard-iron and     |
|                                   | soft-iron values upon completion  |
|                                   || of the turn. To accept the       |
|                                   | parameters, store them using the  |
|                                   || write magnetic calibration       |
|                                   | command.                          |
+-----------------------------------+-----------------------------------+
| 0x000E                            || Write magnetic calibration. The  |
|                                   | unit will write the parameters to |
|                                   || EEPROM and then send a           |
|                                   | calibration response.             |
+-----------------------------------+-----------------------------------+

Calibrate Acknowledgement Response
----------------------------------

+---------------------------+-------------+--------+--------------+-------------+
| Calibrate (‘WC’ = 0x5743) |             |        |              |             |
+---------------------------+-------------+--------+--------------+-------------+
| Preamble                  | Packet Type | Length | Payload      | Termination |
+---------------------------+-------------+--------+--------------+-------------+
| 0x5555                    | 0x5743      | 0x02   | <WC payload> | <CRC (U2)>  |
+---------------------------+-------------+--------+--------------+-------------+

The unit will send this packet in response to a calibrate request if the
procedure can be performed or initiated.

+-----------+-----------+-----------+-----------+-----------+-----------+
| WC        |           |           |           |           |           |
| Payload   |           |           |           |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| Byte      | Name      | Format    | Scaling   | Units     | Descripti |
| Offset    |           |           |           |           | on        |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 0         | calib-    | U2        | -         | -         || The      |
|           | Request   |           |           |           | requested |
|           |           |           |           |           || calibrati|
|           |           |           |           |           | on        |
|           |           |           |           |           | task      |
+-----------+-----------+-----------+-----------+-----------+-----------+

Calibration Completed Parameters Response
-----------------------------------------

+-------------+-------------+-------------+-------------+-------------+
| Calibrate   |             |             |             |             |
| Completed   |             |             |             |             |
| (‘CD’ =     |             |             |             |             |
| 0x4344)     |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| Preamble    | Packet Type | Length      | Payload     | Termination |
+-------------+-------------+-------------+-------------+-------------+
| 0x5555      | 0x4344      | 0x0A        | <CD         | <CRC (U2)>  |
|             |             |             | payload>    |             |
+-------------+-------------+-------------+-------------+-------------+

The unit will send this packet after a calibration has been completed.
Currently, there is only one message of this type sent after a magnetic
calibration has been completed (with or without automatic termination)
and the parameters have been calculated. Thus, the calibrationRequest
field will be 0x000B or 0x000C.

+-----------+-----------+-----------+-----------+-----------+-----------+
| CD        |           |           |           |           |           |
| Payload   |           |           |           |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
|| Byte     | Name      | Format    | Scaling   | Units     | Descripti |
|| Offset   |           |           |           |           | on        |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 0         || calib-   | U2        | -         | -         || The      |
|           || Request  |           |           |           | requested |
|           |           |           |           |           || calibrati|
|           |           |           |           |           | on        |
|           |           |           |           |           | task      |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | xHardIron | I2        | 20/2^16   | G         | The x     |
|           |           |           |           |           | hard iron |
|           |           |           |           |           | bias      |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 4         | yHardIron | I2        | 20/2^16   | G         | The y     |
|           |           |           |           |           | hard iron |
|           |           |           |           |           | bias      |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 6         || softIronS| U2        | 2/2^16    | -         || The      |
|           || caleRatio|           |           |           | scaling   |
|           |           |           |           |           | ratio     |
|           |           |           |           |           || between  |
|           |           |           |           |           | the x and |
|           |           |           |           |           | y axis    |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 8         | softIronA | I2        || 2*pi/2^16| Rad       || The soft |
|           | ngle      |           |           |           | iron      |
|           |           |           || (360/2^16| Deg       | phase     |
|           |           |           | )         |           || angle    |
|           |           |           |           |           | between x |
|           |           |           |           |           | and y     |
|           |           |           |           |           | axis      |
+-----------+-----------+-----------+-----------+-----------+-----------+

Error Response
--------------

+-------------+-------------+-------------+-------------+-------------+
| Error       |             |             |             |             |
| Response    |             |             |             |             |
| NAK =       |             |             |             |             |
| 0x1515)     |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| Preamble    | Packet Type | Length      | Payload     | Termination |
+-------------+-------------+-------------+-------------+-------------+
| 0x5555      | 0x1515      | 0x02        | <NAK        | <CRC (U2)>  |
|             |             |             | payload>    |             |
+-------------+-------------+-------------+-------------+-------------+

The unit will send this packet in place of a normal response to a
*faiiledInputPacketType* request if it could not be completed
successfully.

+-----------+-----------+-----------+-----------+-----------+-----------+
| NAK       |           |           |           |           |           |
| Payload   |           |           |           |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
|| Byte     | Name      | Format    | Scaling   | Units     | Descripti |
|| Offset   |           |           |           |           | on        |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 0         || failed   | U2        | -         | -         | the       |
|           || Input    |           |           |           | failed    |
|           || Packet   |           |           |           || request  |
+-----------+-----------+-----------+-----------+-----------+-----------+

Output Packets(polled)
----------------------

The following packet formats are special informational packets which can
be requested using the ‘GP’ command.

Identification Data Packet
--------------------------

+-------------+-------------+-------------+-------------+-------------+
| Identificat |             |             |             |             |
| ion         |             |             |             |             |
| Data (‘ID’  |             |             |             |             |
| = 0x4944)   |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| Preamble    | Packet Type | Length      | Payload     | Termination |
+-------------+-------------+-------------+-------------+-------------+
| 0x5555      | 0x4944      | 5+N         | <ID         | <CRC (U2)>  |
|             |             |             | payload>    |             |
+-------------+-------------+-------------+-------------+-------------+

This packet contains the unit *serialNumber* and *modelString*. The
model string is terminated with 0x00. The model string contains the
programmed versionString (8-bit Ascii values) followed by the firmware
part number string delimited by a whitespace.

+---------------------+--------------+--------+---------+-------+---------------------+
| ID Payload Contents |              |        |         |       |                     |
+---------------------+--------------+--------+---------+-------+---------------------+
| Byte Offset         | Name         | Format | Scaling | Units | Description         |
+---------------------+--------------+--------+---------+-------+---------------------+
| 0                   | serialNumber | U4     | -       | -     | Unit serial number  |
+---------------------+--------------+--------+---------+-------+---------------------+
| 4                   | modelString  | SN     | -       | -     | Unit Version String |
+---------------------+--------------+--------+---------+-------+---------------------+
| 4+N                 | 0x00         | U1     | -       | -     | Zero Delimiter      |
+---------------------+--------------+--------+---------+-------+---------------------+

Version Data Packet
-------------------

+------------------------------+-------------+--------+--------------+-------------+
| Version Data (‘VR’ = 0x5652) |             |        |              |             |
+------------------------------+-------------+--------+--------------+-------------+
| Preamble                     | Packet Type | Length | Payload      | Termination |
+------------------------------+-------------+--------+--------------+-------------+
| 0x5555                       | 0x5652      | 5      | <VR payload> | <CRC (U2)>  |
+------------------------------+-------------+--------+--------------+-------------+

This packet contains firmware version information. *majorVersion*
changes may introduce serious incompatibilities. *minorVersion* changes
may add or modify functionality, but maintain backward compatibility
with previous minor versions. *patch* level changes reflect bug fixes
and internal modifications with little effect on the user. The build
*stage* is one of the following: 0=release candidate, 1=development,
2=alpha, 3=beta. The *buildNumber* is incremented with each engineering
firmware build. The *buildNumber* and *stage* for released firmware are
both zero. The final beta candidate is v.w.x.3.y, which is then changed
to v.w.x.0.1 to create the first release candidate. The last release
candidate is v.w.x.0.z, which is then changed to v.w.x.0.0 for release.

+-----------+-----------+-----------+-----------+-----------+-------------+
| VR        |           |           |           |           |             |
| Payload   |           |           |           |           |             |
| Contents  |           |           |           |           |             |
+-----------+-----------+-----------+-----------+-----------+-------------+
| Byte      | Name      | Format    | Scaling   | Units     | Descripti   |
| Offset    |           |           |           |           | on          |
+-----------+-----------+-----------+-----------+-----------+-------------+
| 0         | majorVers | U1        | -         | -         | Major       |
|           | ion       |           |           |           | firmware    |
|           |           |           |           |           | version     |
+-----------+-----------+-----------+-----------+-----------+-------------+
| 1         | minorVers | U1        | -         | -         | Minor       |
|           | ion       |           |           |           | firmware    |
|           |           |           |           |           | version     |
+-----------+-----------+-----------+-----------+-----------+-------------+
| 2         | patch     | U1        | -         | -         | Patch       |
|           |           |           |           |           | level       |
+-----------+-----------+-----------+-----------+-----------+-------------+
| 3         | stage     | -         | -         | -         || Development|
|           |           |           |           |           || Stage      |
|           |           |           |           |           || (0=release |
|           |           |           |           |           || candidate, |
|           |           |           |           |           || 1=develop, |
|           |           |           |           |           || 2=alpha,   |
|           |           |           |           |           || 3=beta)    |
+-----------+-----------+-----------+-----------+-----------+-------------+
| 4         | buildNumb | U1        | -         | -         | Build       |
|           | er        |           |           |           | number      |
+-----------+-----------+-----------+-----------+-----------+-------------+

Test 0 (Detailed BIT and Status) Packet
---------------------------------------

+----------------------+-------------+--------+--------------+-------------+
| Test (‘T0’ = 0x5430) |             |        |              |             |
+----------------------+-------------+--------+--------------+-------------+
| Preamble             | Packet Type | Length | Payload      | Termination |
+----------------------+-------------+--------+--------------+-------------+
| 03.3x5555            | 0x5430      | 0x1C   | <T0 payload> | <CRC (U2)>  |
+----------------------+-------------+--------+--------------+-------------+

This packet contains detailed BIT and status information. The full BIT
Status details are described in Section `9 <\l>`__ of this manual.

+-----------+-----------+-----------+-----------+-----------+-----------+
| T0        |           |           |           |           |           |
| Payload   |           |           |           |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| Byte      | Name      | Format    | Scaling   | Units     | Descripti |
| Offset    |           |           |           |           | on        |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 0         | BITstatus | U2        | -         | -         | Master    |
|           |           |           |           |           | BIT and   |
|           |           |           |           |           | Status    |
|           |           |           |           |           | Field     |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         || hardware | U2        | -         | -         | Hardware  |
|           || BIT      |           |           |           | BIT Field |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 4         || hardware | U2        | -         | -         | Hardware  |
|           || PowerBIT |           |           |           | Power BIT |
|           |           |           |           |           | Field     |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 6         || hardware | U2        | -         | -         | Hardware  |
|           || Env BIT  |           |           |           | Environme |
|           |           |           |           |           | BIT Field |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 8         | comBIT    | U2        | -         | -         | communica |
|           |           |           |           |           | tion      |
|           |           |           |           |           | BIT Field |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 10        || comSerial| U2        | -         | -         | Communica |
|           || ABIT     |           |           |           | tion      |
|           |           |           |           |           | Serial A  |
|           |           |           |           |           | BIT Field |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 12        || comSerial| U2        | -         | -         | Communica |
|           || BBIT     |           |           |           | tion      |
|           |           |           |           |           | Serial B  |
|           |           |           |           |           | BIT Field |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 14        || software | U2        | -         | -         | Software  |
|           || BIT      |           |           |           | BIT Field |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 16        || software | U2        | -         | -         | Software  |
|           || Algorithm|           |           |           | Algorithm |
|           || BIT      |           |           |           | BIT Field |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 18        || software | U2        | -         | -         | Software  |
|           || DataBIT  |           |           |           | Data BIT  |
|           |           |           |           |           | Field     |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 20        || hardware | U2        | -         | -         | Hardware  |
|           || status   |           |           |           | Status    |
|           |           |           |           |           | Field     |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 22        | comStatus | U2        | -         | -         | Communica |
|           |           |           |           |           | tion      |
|           |           |           |           |           | Status    |
|           |           |           |           |           | Field     |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 24        || software | U2        | -         | -         | Software  |
|           || status   |           |           |           | Status    |
|           |           |           |           |           | Field     |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 26        || sensor   | U2        | -         | -         | Sensor    |
|           || status   |           |           |           | Status    |
|           |           |           |           |           | Field     |
+-----------+-----------+-----------+-----------+-----------+-----------+

Output Packets(Polled or Continuous)
------------------------------------

Scaled Sensor Data Packet 0
---------------------------

+-------------+-------------+-------------+-------------+-------------+
| Scaled      |             |             |             |             |
| Sensor Data |             |             |             |             |
| (‘S0’ =     |             |             |             |             |
| 0x5330)     |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| Preamble    | Packet Type | Length      | Payload     | Termination |
+-------------+-------------+-------------+-------------+-------------+
| 0x5555      | 0x5330      | 0x1E        | <S0         | <CRC (U2)>  |
|             |             |             | payload>    |             |
+-------------+-------------+-------------+-------------+-------------+

This packet contains scaled sensor data. The scaled sensor data is fixed
point, 2 bytes per sensor, MSB first, for 13 sensors in the following
order: accels(x,y,z); gyros(x,y,z); mags(x,y,z); temps(x,y,z,board).
Data involving angular measurements include the factor pi in the scaling
and can be interpreted in either radians or degrees.

Angular rates: scaled to range of 3.5\* [-pi,+pi) or [-630 deg/sec to
+630 deg/sec)

Accelerometers: scaled to a range of [-10,+10) g

Magnetometers: scaled to a range of [-1,+1) Gauss

Temperature: scaled to a range of [-100, +100)°C

+-----------+-----------+-----------+-----------+-----------+-----------+
| S0        |           |           |           |           |           |
| Payload   |           |           |           |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| Byte      | Name      | Format    | Scaling   | Units     | Descripti |
| Offset    |           |           |           |           | on        |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 0         | xAccel    | I2        | 20/2^16   | g         | X         |
|           |           |           |           |           | accelerom |
|           |           |           |           |           | eter      |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | yAccel    | I2        | 20/2^16   | g         | Y         |
|           |           |           |           |           | accelerom |
|           |           |           |           |           | eter      |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 4         | zAccel    | I2        | 20/2^16   | g         | Z         |
|           |           |           |           |           | accelerom |
|           |           |           |           |           | eter      |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 6         | xRate     | I2        | 7*pi/2^16 | rad/s     | X angular |
|           |           |           |           |           | rate      |
|           |           |           | [1260°/2^ | [°/sec]   |           |
|           |           |           | 16]       |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 8         | yRate     | I2        | 7*pi/2^16 | rad/s     | Y angular |
|           |           |           |           |           | rate      |
|           |           |           | [1260°/2^ | [°/sec]   |           |
|           |           |           | 16]       |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 10        | zRate     | I2        | 7*pi/2^16 | rad/s     | Z angular |
|           |           |           |           |           | rate      |
|           |           |           | [1260°/2^ | [°/sec]   |           |
|           |           |           | 16]       |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 12        | xMag      | I2        | 20/2^16   | Gauss     | X         |
|           |           |           |           |           | magnetome |
|           |           |           |           |           | ter       |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 14        | yMag      | I2        | 20/2^16   | Gauss     | Y         |
|           |           |           |           |           | magnetome |
|           |           |           |           |           | ter       |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 16        | zMag      | I2        | 20/2^16   | Gauss     | Z         |
|           |           |           |           |           | magnetome |
|           |           |           |           |           | ter       |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 18        | xRateTemp | I2        | 200/2^16  | deg. C    | X rate    |
|           |           |           |           |           | temperatu |
|           |           |           |           |           | re        |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 20        | yRateTemp | I2        | 200/2^16  | deg. C    | Y rate    |
|           |           |           |           |           | temperatu |
|           |           |           |           |           | re        |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 22        | zRateTemp | I2        | 200/2^16  | deg. C    | Z rate    |
|           |           |           |           |           | temperatu |
|           |           |           |           |           | re        |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 24        | boardTemp | I2        | 200/2^16  | deg. C    | CPU board |
|           |           |           |           |           | temperatu |
|           |           |           |           |           | re        |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 26        | GPSITOW   | U2        | truncated | ms        | GPS ITOW  |
|           |           |           |           |           | (lower 2  |
|           |           |           |           |           | bytes)    |
|           |           |           |           |           |           |
|           |           |           |           |           | Not       |
|           |           |           |           |           | Implement |
|           |           |           |           |           | ed        |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 28        | BITstatus | U2        | -         | -         | Master    |
|           |           |           |           |           | BIT and   |
|           |           |           |           |           | Status    |
+-----------+-----------+-----------+-----------+-----------+-----------+

Scaled Sensor Data Packet 1 (Default IMU Data) 
----------------------------------------------

+-------------+-------------+-------------+-------------+-------------+
| Scaled      |             |             |             |             |
| Sensor Data |             |             |             |             |
| (‘S1’ =     |             |             |             |             |
| 0x5331)     |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| Preamble    | Packet Type | Length      | Payload     | Termination |
+-------------+-------------+-------------+-------------+-------------+
| 0x5555      | 0x5331      | 0x18        | <S1         | <CRC (U2)>  |
|             |             |             | payload>    |             |
+-------------+-------------+-------------+-------------+-------------+

This packet contains scaled sensor data. Data involving angular
measurements include the factor pi in the scaling and can be interpreted
in either radians or degrees.

Angular rates: scaled to range of 3.5\* [-pi,+pi) or [-630 deg/sec to
+630 deg/sec)

Accelerometers: scaled to a range of [-10,+10)g

Temperature: scaled to a range of [-100, +100)°C

+-----------+-----------+-----------+-----------+-----------+---------------+
| S1        |           |           |           |           |               |
| Payload   |           |           |           |           |               |
+-----------+-----------+-----------+-----------+-----------+---------------+
| Byte      | Name      | Format    | Scaling   | Units     | Description   |
| Offset    |           |           |           |           |               |
+-----------+-----------+-----------+-----------+-----------+---------------+
| 0         | xAccel    | I2        | 20/2^16   | g         | X             |
|           |           |           |           |           | accelerometer |
|           |           |           |           |           |               |
+-----------+-----------+-----------+-----------+-----------+---------------+
| 2         | yAccel    | I2        | 20/2^16   | g         | Y             |
|           |           |           |           |           | accelerometer |
|           |           |           |           |           |               |
+-----------+-----------+-----------+-----------+-----------+---------------+
| 4         | zAccel    | I2        | 20/2^16   | g         | Z             |
|           |           |           |           |           | accelerometer |
|           |           |           |           |           |               |
+-----------+-----------+-----------+-----------+-----------+---------------+
| 6         | xRate     | I2        | 7*pi/2^16 | rad/s     | X angular     |
|           |           |           |           |           | rate          |
|           |           |           | [1260°/2^ | [°/sec]   |               |
|           |           |           | 16]       |           |               |
+-----------+-----------+-----------+-----------+-----------+---------------+
| 8         | yRate     | I2        | 7*pi/2^16 | rad/s     | Y angular     |
|           |           |           |           |           | rate          |
|           |           |           | [1260°/2^ | [°/sec]   |               |
|           |           |           | 16]       |           |               |
+-----------+-----------+-----------+-----------+-----------+---------------+
| 10        | zRate     | I2        | 7*pi/2^16 | rad/s     | Z angular     |
|           |           |           |           |           | rate          |
|           |           |           | [1260°/2^ | [°/sec]   |               |
|           |           |           | 16]       |           |               |
+-----------+-----------+-----------+-----------+-----------+---------------+
| 12        | xRateTemp | I2        | 200/2^16  | deg. C    | X rate        |
|           |           |           |           |           | temperature   |
|           |           |           |           |           |               |
+-----------+-----------+-----------+-----------+-----------+---------------+
| 14        | yRateTemp | I2        | 200/2^16  | deg. C    | Y rate        |
|           |           |           |           |           | temperature   |
|           |           |           |           |           |               |
+-----------+-----------+-----------+-----------+-----------+---------------+
| 16        | zRateTemp | I2        | 200/2^16  | deg. C    | Z rate        |
|           |           |           |           |           | temperature   |
|           |           |           |           |           |               |
+-----------+-----------+-----------+-----------+-----------+---------------+
| 18        | boardTemp | I2        | 200/2^16  | deg. C    | CPU board     |
|           |           |           |           |           | temperature   |
|           |           |           |           |           |               |
+-----------+-----------+-----------+-----------+-----------+---------------+
| 20        | Counter   | U2        | -         | packets   | Output        |
|           |           |           |           |           | packet        |
|           |           |           |           |           | counter       |
+-----------+-----------+-----------+-----------+-----------+---------------+
| 22        | BITstatus | U2        | -         | -         | Master        |
|           |           |           |           |           | BIT and       |
|           |           |           |           |           | Status        |
+-----------+-----------+-----------+-----------+-----------+---------------+

Angle Data Packet 1 (Default AHRS Data)
---------------------------------------

+----------------------------+-------------+--------+--------------+-------------+
| Angle Data (‘A1’ = 0x4131) |             |        |              |             |
+----------------------------+-------------+--------+--------------+-------------+
| Preamble                   | Packet Type | Length | Payload      | Termination |
+----------------------------+-------------+--------+--------------+-------------+
| 0x5555                     | 0x4131      | 0x20   | <A1 payload> | <CRC (U2)>  |
+----------------------------+-------------+--------+--------------+-------------+

This packet contains angle data and selected sensor data scaled in most
cases to a signed 2^16 2’s complement number. Data involving angular
measurements include the factor pi in the scaling and can be interpreted
in either radians or degrees.

Angles: scaled to a range of [-pi,+pi) or [-180 deg to +180 deg).

Angular rates: scaled to range of 3.5\* [-pi,+pi) or [-630 deg/sec to
+630 deg/sec)

Accelerometers: scaled to a range of [-10,+10) g

Magnetometers: scaled to a range of [-10,+10) Gauss

Temperature: scaled to a range of [-100, +100) °C

+-----------+-----------+-----------+-----------+-----------+----------------+
| A1        |           |           |           |           |                |
| Payload   |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| Byte      | Name      | Format    | Scaling   | Units     | Description    |
| Offset    |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 0         | rollAngle | I2        | 2*pi/2^16 | Radians   | Roll           |
|           |           |           |           |           | angle          |
|           |           |           | [360°/2^1 | [°]       |                |
|           |           |           | 6]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 2         | pitchAngl | I2        | 2*pi/2^16 | Radians   | Pitch          |
|           | e         |           |           |           | angle          |
|           |           |           | [360°/2^1 | [°]       |                |
|           |           |           | 6]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 4         || yawAngle | I2        | 2*pi/2^16 | Radians   || Yaw angle     |
|           || Mag      |           |           |           | (magnetic      |
|           |           |           | [360°/2^1 | [°]       || north)        |
|           |           |           | 6]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 6         || xRate    | I2        | 7*pi/2^16 | rad/s     | X angular      |
|           || Corrected|           |           |           | rate           |
|           |           |           | [1260°/2^ | [°/sec]   | Corrected      |
|           |           |           | 16]       |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 8         || yRate    | I2        | 7*pi/2^16 | rad/s     | Y angular      |
|           || Corrected|           |           |           | rate           |
|           |           |           | [1260°/2^ | [°/sec]   | Corrected      |
|           |           |           | 16]       |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 10        || zRate    | I2        | 7*pi/2^16 | rad/s     | Z angular      |
|           || Corrected|           |           |           | rate           |
|           |           |           | [1260°/2^ | [°/sec]   | Corrected      |
|           |           |           | 16]       |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 12        | xAccel    | I2        | 20/2^16   | g         | X              |
|           |           |           |           |           | accelerometer  |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 14        | yAccel    | I2        | 20/2^16   | g         | Y              |
|           |           |           |           |           | accelerometer  |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 16        | zAccel    | I2        | 20/2^16   | g         | Z              |
|           |           |           |           |           | accelerometer  |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 18        | xMag      | I2        | 20/2^16   | Gauss     | X              |
|           |           |           |           |           | magnetometer   |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 20        | yMag      | I2        | 20/2^16   | Gauss     | Y              |
|           |           |           |           |           | magnetometer   |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 22        | zMag      | I2        | 20/2^16   | Gauss     | Z              |
|           |           |           |           |           | magnetometer   |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 24        | xRateTemp | I2        | 200/2^16  | Deg C     | X rate         |
|           |           |           |           |           | temperature    |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 26        | timeITOW  | U4        | 1         | ms        || DMU ITOW      |
|           |           |           |           |           | (sync to       |
|           |           |           |           |           | GPS)           |
|           |           |           |           |           |                |
|           |           |           |           |           || Not           |
|           |           |           |           |           | Implemented    |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 30        | BITstatus | U2        | -         | -         | Master         |
|           |           |           |           |           | BIT and        |
|           |           |           |           |           | Status         |
+-----------+-----------+-----------+-----------+-----------+----------------+

Angle Data Packet 2 (Default VG Data)
-------------------------------------

+----------------------------+-------------+--------+--------------+-------------+
| Angle Data (‘A2’ = 0x4132) |             |        |              |             |
+----------------------------+-------------+--------+--------------+-------------+
| Preamble                   | Packet Type | Length | Payload      | Termination |
+----------------------------+-------------+--------+--------------+-------------+
| 0x5555                     | 0x4132      | 0x1E   | <A2 payload> | <CRC (U2)>  |
+----------------------------+-------------+--------+--------------+-------------+

This packet contains angle data and selected sensor data scaled in most
cases to a signed 2^16 2’s complement number. Data involving angular
measurements include the factor pi in the scaling and can be interpreted
in either radians or degrees.

Angles: scaled to a range of [-pi,+pi) or [-180 deg to +180 deg).

Angular rates: scaled to range of 3.5\* [-pi,+pi) or [-630 deg/sec to
+630 deg/sec)

Accelerometers: scaled to a range of [-10,+10) g

Temperature: scaled to a range of [-100, +100) °C

+-----------+-----------+-----------+-----------+-----------+----------------+
| A2        |           |           |           |           |                |
| Payload   |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| Byte      | Name      | Format    | Scaling   | Units     | Description    |
| Offset    |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 0         | rollAngle | I2        | 2*pi/2^16 | Radians   | Roll           |
|           |           |           |           |           | angle          |
|           |           |           | [360°/2^1 | [°]       |                |
|           |           |           | 6]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 2         | pitchAngl | I2        | 2*pi/2^16 | Radians   | Pitch          |
|           | e         |           |           |           | angle          |
|           |           |           | [360°/2^1 | [°]       |                |
|           |           |           | 6]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 4         || yawAngle | I2        | 2*pi/2^16 | Radians   | Yaw angle      |
|           || True     |           |           |           | (free)         |
|           |           |           | [360°/2^1 | [°]       |                |
|           |           |           | 6]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 6         || xRate    | I2        | 7*pi/2^16 | rad/s     || X angular     |
|           || Corrected|           |           |           | rate           |
|           |           |           | [1260°/2^ | [°/sec]   || corrected     |
|           |           |           | 16]       |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 8         || yRate  r | I2        | 7*pi/2^16 | rad/s     || Y angular     |
|           || Corrected|           |           |           |  rate          |
|           |           |           | [1260°/2^ | [°/sec]   || corrected     |
|           |           |           | 16]       |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 10        || zRate    | I2        | 7*pi/2^16 | rad/s     || Z angular     |
|           || Corrected|           |           |           | rate           |
|           |           |           | [1260°/2^ | [°/sec]   || corrected     |
|           |           |           | 16]       |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 12        | xAccel    | I2        | 20/2^16   | g         || X             |
|           |           |           |           |           || accelerometer |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 14        | yAccel    | I2        | 20/2^16   | g         || Y             |
|           |           |           |           |           || accelerometer |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 16        | zAccel    | I2        | 20/2^16   | g         || Z             |
|           |           |           |           |           || accelerometer |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 18        | xRateTemp | I2        | 200/2^16  | deg. C    || X rate        |
|           |           |           |           |           || temperature   |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 20        | yRateTemp | I2        | 200/2^16  | deg. C    || Y rate        |
|           |           |           |           |           || temperature   |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 22        | zRateTemp | I2        | 200/2^16  | deg. C    || Z rate        |
|           |           |           |           |           || temperature   |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 24        | timeITOW  | U4        | 1         | ms        || DMU ITOW      |
|           |           |           |           |           | (sync to       |
|           |           |           |           |           | GPS)           |
|           |           |           |           |           |                |
|           |           |           |           |           || Not           |
|           |           |           |           |           | Implemented    |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 28        | BITstatus | U2        | -         | -         || Master        |
|           |           |           |           |           | BIT and        |
|           |           |           |           |           || Status        |
+-----------+-----------+-----------+-----------+-----------+----------------+

Angle Data Packet 3 (Default MTLT Data)
---------------------------------------

+----------------------------+-------------+--------+--------------+-------------+
| Angle Data (‘A3’ = 0x4133) |             |        |              |             |
+----------------------------+-------------+--------+--------------+-------------+
| Preamble                   | Packet Type | Length | Payload      | Termination |
+----------------------------+-------------+--------+--------------+-------------+
| 0x5555                     | 0x4133      | 0x1E   | <A3 payload> | <CRC (U2)>  |
+----------------------------+-------------+--------+--------------+-------------+

This packet contains angle data and selected sensor data scaled in most
cases to a signed 2^16 2’s complement number. Data involving angular
measurements include the factor pi in the scaling and can be interpreted
in either radians or degrees.

Angles: scaled to a range of [-pi,+pi) or [-180 deg to +180 deg).

Angular rates: scaled to range of 3.5\* [-pi,+pi) or [-630 deg/sec to
+630 deg/sec)

Accelerometers: scaled to a range of [-10,+10) g

Temperature: scaled to a range of [-100, +100) °C

+-----------+-----------+-----------+-----------+-----------+----------------+
| A3        |           |           |           |           |                |
| Payload   |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| Byte      | Name      | Format    | Scaling   | Units     | Description    |
| Offset    |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 0         | rollAngle | I2        | 2*pi/2^16 | Radians   | Roll           |
|           |           |           |           |           | angle          |
|           |           |           | [360°/2^1 | [°]       |                |
|           |           |           | 6]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 2         | pitchAngle| I2        | 2*pi/2^16 | Radians   | Pitch          |
|           |           |           |           |           | angle          |
|           |           |           | [360°/2^1 | [°]       |                |
|           |           |           | 6]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 4         || yawAngle | I2        | 2*pi/2^16 | Radians   | Yaw angle      |
|           || True     |           |           |           | (free)         |
|           |           |           | [360°/2^1 | [°]       |                |
|           |           |           | 6]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 6         || xRate    | I2        | 7*pi/2^16 | rad/s     || X angular     |
|           || Scaled   |           |           |           | rate           |
|           |           |           | [1260°/2^ | [°/sec]   || corrected     |
|           |           |           | 16]       |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 8         || yRate    | I2        | 7*pi/2^16 | rad/s     || Y angular     |
|           || Scaled   |           |           |           | rate           |
|           |           |           | [1260°/2^ | [°/sec]   || corrected     |
|           |           |           | 16]       |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 10        || zRate    | I2        | 7*pi/2^16 | rad/s     || Z angular     |
|           || Scaled   |           |           |           | rate           |
|           |           |           | [1260°/2^ | [°/sec]   || corrected     |
|           |           |           | 16]       |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 12        | xAccel    | I2        | 20/2^16   | g         || X             |
|           |           |           |           |           || accelerometer |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 14        | yAccel    | I2        | 20/2^16   | g         || Y             |
|           |           |           |           |           || accelerometer |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 16        | zAccel    | I2        | 20/2^16   | g         || Z             |
|           |           |           |           |           || accelerometer |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 18        | xRateTemp | I2        | 200/2^16  | deg. C    || X rate        |
|           |           |           |           |           || temperature   |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 20        | yRateTemp | I2        | 200/2^16  | deg. C    || Y rate        |
|           |           |           |           |           || temperature   |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 22        | zRateTemp | I2        | 200/2^16  | deg. C    || Z rate        |
|           |           |           |           |           || temperature   |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 24        | timeITOW  | U4        | 1         | ms        || DMU ITOW      |
|           |           |           |           |           | (sync to       |
|           |           |           |           |           | GPS)           |
|           |           |           |           |           |                |
|           |           |           |           |           || Not           |
|           |           |           |           |           | Implemented    |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 28        | BITstatus | U2        | -         | -         || Master        |
|           |           |           |           |           | BIT and        |
|           |           |           |           |           || Status        |
+-----------+-----------+-----------+-----------+-----------+----------------+

Nav Data Packet 0
-----------------

+--------------------------+-------------+--------+--------------+-------------+
| Nav Data (‘N0’ = 0x4E30) |             |        |              |             |
+--------------------------+-------------+--------+--------------+-------------+
| Preamble                 | Packet Type | Length | Payload      | Termination |
+--------------------------+-------------+--------+--------------+-------------+
| 0x5555                   | 0x4E30      | 0x20   | <N0 payload> | <CRC (U2)>  |
+--------------------------+-------------+--------+--------------+-------------+

This packet contains navigation data and selected sensor data scaled in
most cases to a signed 2^16 2’s complement number. Data involving
angular measurements include the factor pi in the scaling and can be
interpreted in either radians or degrees.

Angles: scaled to a range of [-pi,+pi) or [-180 deg to +180 deg).

Angular rates: scaled to range of 3.5\* [-pi,+pi) or [-630 deg/sec to
+630 deg/sec)

Accelerometers: scaled to a range of [-10,+10) g

Temperature: scaled to a range of [-100, +100) °C

Velocities are scaled to a range of [-256,256) m/s

    Altitude is scaled to a range of [-100,16284) m using a shifted 2’s
    complement representation.

Longitude and latitude are scaled to a range of [-pi,pi) or [-180 deg to
+180 deg).

+-----------+-----------+-----------+-----------+-----------+----------------+
| N0        |           |           |           |           |                |
| Payload   |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| Byte      | Name      | Format    | Scaling   | Units     | Description    |
| Offset    |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 0         | rollAngle | I2        | 2*pi/2^16 | Radians   | Roll           |
|           |           |           |           |           | angle          |
|           |           |           | [360°/2^1 | [°]       |                |
|           |           |           | 6]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 2         | pitchAngle| I2        | 2*pi/2^16 | Radians   | Pitch          |
|           |           |           |           |           | angle          |
|           |           |           | [360°/2^1 | [°]       |                |
|           |           |           | 6]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 4         || yawAngle | I2        | 2*pi/2^16 | Radians   || Yaw angle     |
|           || True     |           |           |           || (true         |
|           |           |           | [360°/2^1 | [°]       | north)         |
|           |           |           | 6]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 6         || xRate    | I2        | 7*pi/2^16 | rad/s     || X angular     |
|           || Corrected|           |           |           | rate           |
|           |           |           | [1260°/2^ | [°/sec]   || corrected     |
|           |           |           | 16]       |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 8         || yRate    | I2        | 7*pi/2^16 | rad/s     || Y angular     |
|           || Corrected|           |           |           | rate           |
|           |           |           | [1260°/2^ | [°/sec]   || corrected     |
|           |           |           | 16]       |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 10        || zRate    | I2        | 7*pi/2^16 | rad/s     || Z angular     |
|           || Corrected|           |           |           | rate           |
|           |           |           | [1260°/2^ | [°/sec]   || corrected     |
|           |           |           | 16]       |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 12        | nVel      | I2        | 512/2^16  | m/s       | North          |
|           |           |           |           |           | velocity       |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 14        | eVel      | I2        | 512/2^16  | m/s       | East           |
|           |           |           |           |           | velocity       |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 16        | dVel      | I2        | 512/2^16  | m/s       | Down           |
|           |           |           |           |           | velocity       |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 18        | longitude | I4        | 2*pi/2^32 | Radians   | Longitude      |
|           |           |           |           |           |                |
|           |           |           | [360°/2^3 | [°]       |                |
|           |           |           | 2]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 22        | latitude  | I4        | 2*pi/2^32 | Radians   | Latitude       |
|           |           |           |           |           |                |
|           |           |           | [360°/2^3 | [°]       |                |
|           |           |           | 2]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 26        | altitude  | I2\*      | 2^14/2^16 | m         || GPS           |
|           |           |           |           |           | altitude       |
|           |           |           |           |           || [-100,162     |
|           |           |           |           |           | 84)            |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 28        | ITOW      | U2        | truncated | ms        || ITOW          |
|           |           |           |           |           || (lower 2      |
|           |           |           |           |           | bytes)         |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 30        | BITstatus | U2        | -         | -         || Master        |
|           |           |           |           |           | BIT and        |
|           |           |           |           |           || Status        |
+-----------+-----------+-----------+-----------+-----------+----------------+

Nav Data Packet 1 (Default INS Data)
------------------------------------

+--------------------------+-------------+--------+--------------+-------------+
| Nav Data (‘N1’ = 0x4E31) |             |        |              |             |
+--------------------------+-------------+--------+--------------+-------------+
| Preamble                 | Packet Type | Length | Payload      | Termination |
+--------------------------+-------------+--------+--------------+-------------+
| 0x5555                   | 0x4E31      | 0x2A   | <N1 payload> | <CRC (U2)>  |
+--------------------------+-------------+--------+--------------+-------------+

This packet contains navigation data and selected sensor data scaled in
most cases to a signed 2^16 2’s complement number. Data involving
angular measurements include the factor pi in the scaling and can be
interpreted in either radians or degrees.

Angles: scaled to a range of [-pi,+pi) or [-180 deg to +180 deg).

Angular rates: scaled to range of 3.5\* [-pi,+pi) or [-630 deg/sec to
+630 deg/sec)

Accelerometers: scaled to a range of [-10,+10) g

Temperature: scaled to a range of [-100, +100) °C

Velocities are scaled to a range of [-256,256) m/s

    Altitude is scaled to a range of [-100,16284) m using a shifted 2’s
    complement representation.

Longitude and latitude are scaled to a range of [-pi,pi) or [-180 deg to
+180 deg).

+-----------+-----------+-----------+-----------+-----------+----------------+
| N1        |           |           |           |           |                |
| Payload   |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| Byte      | Name      | Format    | Scaling   | Units     | Description    |
| Offset    |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 0         | rollAngle | I2        | 2*pi/2^16 | Radians   | Roll           |
|           |           |           |           |           | angle          |
|           |           |           | [360°/2^1 | [°]       |                |
|           |           |           | 6]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 2         | pitchAngle| I2        | 2*pi/2^16 | Radians   | Pitch          |
|           |           |           |           |           | angle          |
|           |           |           | [360°/2^1 | [°]       |                |
|           |           |           | 6]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 4         || yawAngle | I2        | 2*pi/2^16 | Radians   || Yaw angle     |
|           || True     |           |           |           || (true         |
|           |           |           | [360°/2^1 | [°]       | north)         |
|           |           |           | 6]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 6         || xRate    | I2        | 7*pi/2^16 | rad/s     || X angular     |
|           || Corrected|           |           |           | rate           |
|           |           |           | [1260°/2^ | [°/sec]   || corrected     |
|           |           |           | 16]       |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 8         || yRate    | I2        | 7*pi/2^16 | rad/s     || Y angular     |
|           || Corrected|           |           |           | rate           |
|           |           |           | [1260°/2^ | [°/sec]   || corrected     |
|           |           |           | 16]       |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 10        || zRate    | I2        | 7*pi/2^16 | rad/s     || Z angular     |
|           || Corrected|           |           |           | rate           |
|           |           |           | [1260°/2^ | [°/sec]   || corrected     |
|           |           |           | 16]       |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 12        | xAccel    | I2        | 20/2^16   | g         || X             |
|           |           |           |           |           || accelerometer |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 14        | yAccel    | I2        | 20/2^16   | g         || Y             |
|           |           |           |           |           || accelerometer |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 16        | zAccel    | I2        | 20/2^16   | g         || Z             |
|           |           |           |           |           || accelerometer |
|           |           |           |           |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 18        | nVel      | I2        | 512/2^16  | m/s       | North          |
|           |           |           |           |           | velocity       |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 20        | eVel      | I2        | 512/2^16  | m/s       | East           |
|           |           |           |           |           | velocity       |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 22        | dVel      | I2        | 512/2^16  | m/s       | Down           |
|           |           |           |           |           | velocity       |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 24        | longitude | I4        | 2*pi/2^32 | Radians   | Longitude      |
|           |           |           |           |           |                |
|           |           |           | [360°/2^3 | [°]       |                |
|           |           |           | 2]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 28        | latitude  | I4        | 2*pi/2^32 | Radians   | Latitude       |
|           |           |           |           |           |                |
|           |           |           | [360°/2^3 | [°]       |                |
|           |           |           | 2]        |           |                |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 32        | altitude  | I2\*      | 2^14/2^16 | m         || Altitude      |
|           |           |           |           |           || [-100,162     |
|           |           |           |           |           | 84)            |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 34        | xRateTemp | I2        | 200/2^16  | deg C     || X rate        |
|           |           |           |           |           | sensor         |
|           |           |           |           |           || temperatu     |
|           |           |           |           |           | re             |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 36        | ITOW      | U4        | 1         | ms        | ITOW           |
|           |           |           |           |           | (sync to       |
|           |           |           |           |           | GPS)           |
+-----------+-----------+-----------+-----------+-----------+----------------+
| 40        | BITstatus | U2        | -         | -         | Master         |
|           |           |           |           |           | BIT and        |
|           |           |           |           |           | Status         |
+-----------+-----------+-----------+-----------+-----------+----------------+


