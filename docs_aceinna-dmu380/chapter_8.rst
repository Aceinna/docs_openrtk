DMUx81ZA Advanced UART Port Commands
************************************

The advanced commands allow users to programmatically change the
DMUx81ZA Series settings. This section of the manual documents all of
the settings and options contained under the Unit Configuration tab
within NAV-VIEW. Using these advanced commands, a user’s system can
change or modify the settings without the need for NAV-VIEW.

Configuration Fields
--------------------

Configuration fields determine various behaviors of the unit that can be
modified by the user. These include settings like baud rate, packet
output rate and type, algorithm type, etc. These fields are stored in
EEPROM and loaded on power up. These fields can be read from the EEPROM
using the ‘RF’ command. These fields can be written to the EEPROM
affecting the default power up behavior using the ‘WF’ command. The
current value of these fields (which may be different from the value
stored in the EEPROM) can also be accessed using the ‘GF’ command. All
of these fields can also be modified immediately for the duration of the
current power cycle using the ‘SF’ command. The unit will always power
up in the configuration stored in the EEPROM. Configuration fields can
only be set or written with valid data from Table 40 below.

            **Table 40 Configuration Fields**

+-----------------+-----------------+-----------------+-----------------+
|| configuration  | field ID        || Valid          |    description  |
|| fields         |                 || Values         |                 |
+-----------------+-----------------+-----------------+-----------------+
|| Packet rate    | 0x0001          || 0,1,2,4,5,10,  || quiet, 100Hz,  |
|| divider        |                 || 20, 25, 50     || 50Hz, 25Hz,    |
|                 |                 |                 || 20Hz, 10Hz,    |
|                 |                 |                 || 5Hz, 4Hz, 2Hz  |
+-----------------+-----------------+-----------------+-----------------+
|| Unit BAUD      | 0x0002          | 2,3,5,6         || 38400, 57600   | 
|| rate           |                 |                 || 115200, 230400 |
+-----------------+-----------------+-----------------+-----------------+
|| Continuous     | 0x0003          || Any output     || Not all output |
|| packet type    |                 || packet type    | packets         |
|                 |                 |                 || available for  |
|                 |                 |                 | all products.   |
|                 |                 |                 || See detailed   |
|                 |                 |                 | product         |
|                 |                 |                 || descriptions.  |
+-----------------+-----------------+-----------------+-----------------+
| Unused          | 0x0004          |                 |                 |
+-----------------+-----------------+-----------------+-----------------+
|| Gyro Filter    | 0x0005          | | 18750-65535   || Sets low pass  |
|| Setting        |                 |   [2Hz]         | cutoff for rate |
|                 |                 | | 8135-18749    || sensors. Cutoff|
|                 |                 |   [5Hz]         | Frequency       |
|                 |                 | | 4018-8134     || choices are 2, |
|                 |                 |   [10Hz]        | 5, 10, 20, 25,  |
|                 |                 | | 2411-4017     || 40, and 50Hz   |
|                 |                 |   [20Hz]        |                 |
|                 |                 | | 1741-2410     |                 |
|                 |                 |   [25Hz]        |                 |
|                 |                 | | 1205-1740     |                 |
|                 |                 |   [40Hz]        |                 |
|                 |                 | | 1-1204 [50    |                 |
|                 |                 |   Hz]           |                 |
|                 |                 |                 |                 |
|                 |                 | 0 [Unfiltered]  |                 |
+-----------------+-----------------+-----------------+-----------------+
|| Accelerometer  | 0x0006          | | 18750-65535   || Sets low pass  |
|| Filter Setting |                 |   [2Hz]         | cutoff for      |
|                 |                 | | 8135-18749    || accelerometers.|
|                 |                 |   [5Hz]         | Cutoff          |
|                 |                 | | 4018-8134     || Frequency      |
|                 |                 |   [10Hz]        | choices are 2,  |
|                 |                 | | 2411-4017     || 5, 10, 20, 25, |
|                 |                 |   [20Hz]        | 40, and 50Hz    |
|                 |                 | | 1741-2410     |                 |
|                 |                 |   [25Hz]        |                 |
|                 |                 | | 1205-1740     |                 |
|                 |                 |   [40Hz]        |                 |
|                 |                 | | 1-1204 [50    |                 |
|                 |                 |   Hz]           |                 |
|                 |                 |                 |                 |
|                 |                 | 0 [Unfiltered]  |                 |
+-----------------+-----------------+-----------------+-----------------+
| Orientation     | 0x0007          | See below       || Determine      |
|                 |                 |                 | forward,        |
|                 |                 |                 || rightward, and |
|                 |                 |                 | downward facing |
|                 |                 |                 || sides          |
+-----------------+-----------------+-----------------+-----------------+
|| User Behavior  | 0x0008          | Any             || Free Integrate |
|| Switches       |                 |                 | (60 sec), Use   |
|                 |                 |                 || Mags, Use GPS, |
|                 |                 |                 | Stationary Yaw  |
|                 |                 |                 || Lock, …        |
+-----------------+-----------------+-----------------+-----------------+
|| X Hard Iron    | 0x0009          | Any             | I2 scaled from  |
|| Bias           |                 |                 | [-1,1)          |
+-----------------+-----------------+-----------------+-----------------+
|| Y Hard Iron    | 0x000A          | Any             | I2 scaled from  |
|| Bias           |                 |                 | [-1,1)          |
+-----------------+-----------------+-----------------+-----------------+
|| Soft Iron      | 0x000B          | Any             | U2 scaled from  |
|| Scale Ratio    |                 |                 | [0,2)           |
+-----------------+-----------------+-----------------+-----------------+
|| Soft Iron      | 0x000E          | Any             | I2 scaled from  |
|| Phase Angle    |                 |                 | [-pi,pi]        |
+-----------------+-----------------+-----------------+-----------------+

Note: BAUD rate SF has immediate effect. Some output data may be lost.
Response will be received at new BAUD rate.

Continuous Packet Type Field
----------------------------

This is the packet type that is being continually output. The supported
packet depends on the model number. Please refer to Section `7.4 <\l>`__
for a complete list of the available packet types.

Digital Filter Settings
-----------------------

These two fields set the digital low pass filter cutoff frequencies (See
Table 41). Each sensor listed is defined in the default factory
orientation. Users must consider any additional rotation to their
intended orientation.

         **Table 41 Digital Filter Settings**

+--------------------+----------------+
| **Filter Setting** | **Sensor**     |
+--------------------+----------------+
| FilterGyro         | Ux,Uy,Uz Rate  |
+--------------------+----------------+
| FilterAccel        | Ux,Uy,Uz Accel |
+--------------------+----------------+

Orientation Field
-----------------

This field defines the rotation from the factory to user axis sets. This
rotation is relative to the default factory orientation for the
appropriate DMUx81 family model. The default factory axis setting for
the IMUx81ZA-200 orientation field is (-Uy, -Ux, -Uz) which defines the
connector pointing in the +Z direction and the +X direction going from
the connector through the serial number label at the end of the DMUx81.
The user axis set (X, Y, Z) as defined by this field setting is depicted
in `Figure 18 <\l>`__ below:

|image12|

Figure 18 IMUx81ZA-200 Default Orientation Field (0x006B)

          **Table 42 DMUx81 Orientation Definitions**

+-------------------+------------+---------------------------------+
| **Description**   | **Bits**   | **Meaning**                     |
+-------------------+------------+---------------------------------+
| X Axis Sign       | 0          | 0 = positive, 1 = negative      |
+-------------------+------------+---------------------------------+
| X Axis            | 1:2        | 0 = Ux, 1 = Uy, 2 = Uz, 3 = N/A |
+-------------------+------------+---------------------------------+
| Y Axis Sign       | 3          | 0 = positive, 1 = negative      |
+-------------------+------------+---------------------------------+
| Y Axis            | 4:5        | 0 = Uy, 1 = Uz, 2 = Ux, 3 = N/A |
+-------------------+------------+---------------------------------+
| Z Axis Sign       | 6          | 0 = positive, 1 = negative      |
+-------------------+------------+---------------------------------+
| Z Axis            | 7:8        | 0 = Uz, 1 = Ux, 2 = Uy, 3 = N/A |
+-------------------+------------+---------------------------------+
| Reserved          | 9:15       | N/A                             |
+-------------------+------------+---------------------------------+

There are 24 possible orientation configurations (See Table 43).
Setting/Writing the field to anything else generates a NAK and has no
effect.

            **Table 43 DMUx81 Orientation Fields**

+-------------------------------+--------------+--------------+--------------+
| ***Orientation Field Value*** | ***X Axis*** | ***Y Axis*** | ***Z Axis*** |
+-------------------------------+--------------+--------------+--------------+
| 0x0000                        | +Ux          | +Uy          | +Uz          |
+-------------------------------+--------------+--------------+--------------+
| 0x0009                        | -Ux          | -Uy          | +Uz          |
+-------------------------------+--------------+--------------+--------------+
| 0x0023                        | -Uy          | +Ux          | +Uz          |
+-------------------------------+--------------+--------------+--------------+
| 0x002A                        | +Uy          | -Ux          | +Uz          |
+-------------------------------+--------------+--------------+--------------+
| 0x0041                        | -Ux          | +Uy          | -Uz          |
+-------------------------------+--------------+--------------+--------------+
| 0x0048                        | +Ux          | -Uy          | -Uz          |
+-------------------------------+--------------+--------------+--------------+
| 0x0062                        | +Uy          | +Ux          | -Uz          |
+-------------------------------+--------------+--------------+--------------+
| 0x006B                        | -Uy          | -Ux          | -Uz          |
+-------------------------------+--------------+--------------+--------------+
| 0x0085                        | -Uz          | +Uy          | +Ux          |
+-------------------------------+--------------+--------------+--------------+
| 0x008C                        | +Uz          | -Uy          | +Ux          |
+-------------------------------+--------------+--------------+--------------+
| 0x0092                        | +Uy          | +Uz          | +Ux          |
+-------------------------------+--------------+--------------+--------------+
| 0x009B                        | -Uy          | -Uz          | +Ux          |
+-------------------------------+--------------+--------------+--------------+
| 0x00C4                        | +Uz          | +Uy          | -Ux          |
+-------------------------------+--------------+--------------+--------------+
| 0x00CD                        | -Uz          | -Uy          | -Ux          |
+-------------------------------+--------------+--------------+--------------+
| 0x00D3                        | -Uy          | +Uz          | -Ux          |
+-------------------------------+--------------+--------------+--------------+
| 0x00DA                        | +Uy          | -Uz          | -Ux          |
+-------------------------------+--------------+--------------+--------------+
| 0x0111                        | -Ux          | +Uz          | +Uy          |
+-------------------------------+--------------+--------------+--------------+
| 0x0118                        | +Ux          | -Uz          | +Uy          |
+-------------------------------+--------------+--------------+--------------+
| 0x0124                        | +Uz          | +Ux          | +Uy          |
+-------------------------------+--------------+--------------+--------------+
| 0x012D                        | -Uz          | -Ux          | +Uy          |
+-------------------------------+--------------+--------------+--------------+
| 0x0150                        | +Ux          | +Uz          | -Uy          |
+-------------------------------+--------------+--------------+--------------+
| 0x0159                        | -Ux          | -Uz          | -Uy          |
+-------------------------------+--------------+--------------+--------------+
| 0x0165                        | -Uz          | +Ux          | -Uy          |
+-------------------------------+--------------+--------------+--------------+
| 0x016C                        | +Uz          | -Ux          | -Uy          |
+-------------------------------+--------------+--------------+--------------+

The default factory axis setting for all other DMUx81 family model’s
orientation field is (+Ux, +Uy, +Uz) which defines the base of the
DMUx81 pointing in the +Z direction and the +Y direction going from the
serial number label at the end through the connector of the DMUx81. The
user axis set (X, Y, Z) as defined by this field setting is depicted in
`Figure 19 <\l>`__ below:

|image13|

User Behavior Switches
----------------------

This field allows on the fly user interaction with behavioral aspects of
the algorithm (See Table 44).

              **Table 44 DMUx81 Behavior Switches**

+-----------------------+-----------------------+-----------------------+
| **Description**       | **Bits**              | **Meaning**           |
+-----------------------+-----------------------+-----------------------+
| Free Integrate        | 0                     || 0 = use feedback to  |
|                       |                       | stabilize the         |
|                       |                       || algorithm, 1 = 6DOF  |
|                       |                       | inertial integration  |
|                       |                       || without stabilized   |
|                       |                       | feedback for 60       |
|                       |                       || seconds              |
+-----------------------+-----------------------+-----------------------+
| Use Mags              | 1                     || 0 = Do not use mags  |
|                       |                       | to stabilize heading  |
|                       |                       || (heading will run    |
|                       |                       | open loop or be       |
|                       |                       || stabilized by GPS    |
|                       |                       | track), 1 = Use mags  |
|                       |                       || to stabilize heading |
+-----------------------+-----------------------+-----------------------+
| Use GPS               | 2                     || 0 = Do not use GPS to|
|                       |                       | stabilize the system, |
|                       |                       || 1 = Use GPS when     |
|                       |                       | available             |
+-----------------------+-----------------------+-----------------------+
| Stationary Yaw Lock   | 3                     || 0 = Do not lock yaw  |
|                       |                       | when GPS speed is     |
|                       |                       || near zero (<0.75     |
|                       |                       | m/s), 1 = Lock yaw    |
|                       |                       || when GPS speed is    |
|                       |                       | near zero             |
+-----------------------+-----------------------+-----------------------+
| Restart on Over-range | 4                     || 0 = Do not restart   |
|                       |                       | the system after a    |
|                       |                       || sensor over-range, 1 |
|                       |                       | = restart the system  |
|                       |                       || after a sensor       |
|                       |                       | over-range            |
+-----------------------+-----------------------+-----------------------+
| Dynamic Motion        | 5                     || 0=vehicle is static, |
|                       |                       | force high gain       |
|                       |                       || corrections, 1=      |
|                       |                       | vehicle is dynamic,   |
|                       |                       || use nominal          |
|                       |                       | corrections           |
+-----------------------+-----------------------+-----------------------+
| Reserved              | 6:15                  | N/A                   |
+-----------------------+-----------------------+-----------------------+

Hard and Soft Iron Values
-------------------------

These fields allow access to hard iron bias and soft iron scale ratio
values for magnetometer alignment (See Table 45):

           **Table 45 DMUx81 Magnetic Alignment Parameters**

+-----------------------+----------------+--------------+---------------+-------------+
| **Field Name**        | **Field ID**   | **Format**   | **Scaling**   | **Units**   |
+-----------------------+----------------+--------------+---------------+-------------+
| X Hard Iron Bias      | 0x0009         | I2           | 2/2^16        | Gauss       |
+-----------------------+----------------+--------------+---------------+-------------+
| Y Hard Iron Bias      | 0x000A         | I2           | 2/2^16        | Gauss       |
+-----------------------+----------------+--------------+---------------+-------------+
| Soft Iron Scale Ratio | 0x000B         | U2           | 2/2^16        | -           |
+-----------------------+----------------+--------------+---------------+-------------+
| Soft Iron Phase Angle | 0x000E         | I2           | 2*pi/2^16     | Radians     |
+-----------------------+----------------+--------------+---------------+-------------+

The hard iron bias values are scaled from [-1,1] Gauss. These values are
subtracted from the tangent plane magnetometer vector before the heading
reference is calculated for the filter. The soft iron scale ratio is
scaled from [0,2] and is multiplied by the tangent plane x magnetometer
value before the heading reference is calculated for the filter. The
soft iron phase angle is scaled from [-pi,pi] and is applied to the
tangent plane x magnetometer value before the heading reference is
calculated for the filter. This compensates for elliptical soft iron
errors that generate an ellipse at an angle away from the major or minor
axis following the full rotation of a magnetometer alignment. Note that
none of these parameters are applied to the output magnetometer vector
data in message A1. They are only applied internally to the data for use
in the heading reference for the Kalman filter.

Heading Track Offset
--------------------

This field is used to set the offset between vehicle heading and vehicle
track to be used by the navigation mode filter when no magnetometer
heading measurements are available (See Table 46).

           **Table 46 DMUx81 Heading Track Offset**

+-------------+-------------+-------------+-------------+-------------+
| **Field     | **Field     | **Format**  | **Scaling** | **Unit**    |
| Name**      | ID**        |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| Heading     | 0x000C      | I2          | 2*pi/2^16   | Radians     |
| Track       |             |             |             | (heading-tr |
| Offset      |             |             | [360°/2^16] | ack)        |
|             |             |             |             |             |
|             |             |             |             | [°]         |
+-------------+-------------+-------------+-------------+-------------+

Commands to Program Configuration
---------------------------------

Write Fields Command
--------------------

+-------------+-------------+-------------+-------------+-------------+
| Write       |             |             |             |             |
| Fields      |             |             |             |             |
| (‘WF’ =     |             |             |             |             |
| 0x5746)     |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| Preamble    | Packet Type | Length      | Payload     | Termination |
+-------------+-------------+-------------+-------------+-------------+
| 0x5555      | 0x5746      | 1+numFields | <WF         | <CRC (U2)>  |
|             |             | *4          | payload>    |             |
+-------------+-------------+-------------+-------------+-------------+

This command allows the user to write default power-up configuration
fields to the EEPROM. Writing the default configuration will not take
affect until the unit is power cycled. *NumFields* is the number of
words to be written. The *field0, field1, etc.* are the field

IDs that will be written with the *field0Data, field1Data, etc.*,
respectively. The unit will not write to calibration or algorithm
fields. If at least one field is successfully written, the unit will
respond with a write fields response containing the field IDs of the
successfully written fields. If any field is unable to be written, the
unit will respond with an error response. Note that both a write fields
and an error response may be received as a result of a write fields
command. Attempts to write a field with an invalid value is one way to
generate an error response. A table of field IDs and valid field values
is available in Section `8.1 <\l>`__.

+-----------+-----------+-----------+-----------+-----------+--------------+
| WF        |           |           |           |           |              |
| Payload   |           |           |           |           |              |
+-----------+-----------+-----------+-----------+-----------+--------------+
| Byte      | Name      | Format    | Scaling   | Units     | Description  |
| Offset    |           |           |           |           |              |
+-----------+-----------+-----------+-----------+-----------+--------------+
| 0         | numFields | U1        | -         | -         || The         |
|           |           |           |           |           | number of    |
|           |           |           |           |           || fields to   |
|           |           |           |           |           | write        |
+-----------+-----------+-----------+-----------+-----------+--------------+
| 1         | field0    | U2        | -         | -         || The first   |
|           |           |           |           |           | field ID     |
|           |           |           |           |           || to write    |
+-----------+-----------+-----------+-----------+-----------+--------------+
| 3         | field0Dat | U2        | -         | -         || The first   |
|           | a         |           |           |           | field        |
|           |           |           |           |           || ID’s data   |
|           |           |           |           |           | to write     |
+-----------+-----------+-----------+-----------+-----------+--------------+
| 5         | field1    | U2        | -         | -         || The         |
|           |           |           |           |           | second       |
|           |           |           |           |           || field ID    |
|           |           |           |           |           | to write     |
+-----------+-----------+-----------+-----------+-----------+--------------+
| 7         | field1Dat | U2        | -         | -         || The         |
|           | a         |           |           |           | second       |
|           |           |           |           |           || field       |
|           |           |           |           |           | ID’s data    |
+-----------+-----------+-----------+-----------+-----------+--------------+
| …         | …         | U2        | -         | -         | …            |
+-----------+-----------+-----------+-----------+-----------+--------------+
| numFields | field…    | U2        | -         | -         || The last    |
| *4        |           |           |           |           | field ID     |
| -3        |           |           |           |           || to write    |
+-----------+-----------+-----------+-----------+-----------+--------------+
| numFields | field…Dat | U2        | -         | -         || The last    |
| *4        | a         |           |           |           | field        |
| -1        |           |           |           |           || ID’s data   |
|           |           |           |           |           | to write     |
+-----------+-----------+-----------+-----------+-----------+--------------+

**Write Fields Response**

+-------------+-------------+-------------+-------------+-------------+
| Write       |             |             |             |             |
| Fields      |             |             |             |             |
| (‘WF’ =     |             |             |             |             |
| 0x5746)     |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| Preamble    | Packet Type | Length      | Payload     | Termination |
+-------------+-------------+-------------+-------------+-------------+
| 0x5555      | 0x5746      | 1+numFields | <WF         | <CRC (U2)>  |
|             |             | *2          | payload>    |             |
+-------------+-------------+-------------+-------------+-------------+

The unit will send this packet in response to a write fields command if
the command has completed without errors.

+-----------+-----------+-----------+-----------+-----------+-----------+
| WF        |           |           |           |           |           |
| Payload   |           |           |           |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| Byte      | Name      | Format    | Scaling   | Units     | Descripti |
| Offset    |           |           |           |           | on        |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 0         | numFields | U1        | -         | -         | The       |
|           |           |           |           |           | number of |
|           |           |           |           |           | fields    |
|           |           |           |           |           | written   |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 1         | field0    | U2        | -         | -         | The first |
|           |           |           |           |           | field ID  |
|           |           |           |           |           | written   |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | field1    | U2        | -         | -         | The       |
|           |           |           |           |           | second    |
|           |           |           |           |           | field ID  |
|           |           |           |           |           | written   |
+-----------+-----------+-----------+-----------+-----------+-----------+
| …         | …         | U2        | -         | -         | More      |
|           |           |           |           |           | field IDs |
|           |           |           |           |           | written   |
+-----------+-----------+-----------+-----------+-----------+-----------+
| numFields | Field…    | U2        | -         | -         | The last  |
| *2        |           |           |           |           | field ID  |
| – 1       |           |           |           |           | written   |
+-----------+-----------+-----------+-----------+-----------+-----------+

Set Fields Command
------------------

+-------------+-------------+-------------+-------------+-------------+
| Set Fields  |             |             |             |             |
| (‘SF’ =     |             |             |             |             |
| 0x5346)     |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| Preamble    | Packet Type | Length      | Payload     | Termination |
+-------------+-------------+-------------+-------------+-------------+
| 0x5555      | 0x5346      | 1+numFields | <SF         | <CRC (U2)>  |
|             |             | *4          | payload>    |             |
+-------------+-------------+-------------+-------------+-------------+

This command allows the user to set the unit’s current configuration
(SF) fields immediately which will then be lost on power down.
*NumFields* is the number of words to be set. The *field0, field1, etc.*
are the field IDs that will be written with the *field0Data, field1Data,
etc.*, respectively. This command can be used to set configuration
fields. The unit will not set calibration or algorithm fields. If at
least one field is successfully set, the unit will respond with a set
fields response containing the field IDs of the successfully set fields.
If any field is unable to be set, the unit will respond with an error
response. Note that both a set fields and an error response may be
received as a result of one set fields command. Attempts to set a field
with an invalid value is one way to generate an error response. A table
of field IDs and valid field values is available in Section
`8.1 <\l>`__.

+-----------+-----------+-----------+-----------+-----------+-----------+
| SF        |           |           |           |           |           |
| Payload   |           |           |           |           |           |
| Contents  |           |           |           |           |           | 
+-----------+-----------+-----------+-----------+-----------+-----------+
| *Byte     | *Name*    | *Format*  | *Scaling* | *Units*   | *Descript |
| Offset*   |           |           |           |           | ion*      |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 0         | numFields | U1        | -         | -         | The       |
|           |           |           |           |           | number of |
|           |           |           |           |           | fields to |
|           |           |           |           |           | set       |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 1         | field0    | U2        | -         | -         | The first |
|           |           |           |           |           | field ID  |
|           |           |           |           |           | to set    |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | field0Dat | U2        | -         | -         | The first |
|           | a         |           |           |           | field     |
|           |           |           |           |           | ID’s data |
|           |           |           |           |           | to set    |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 5         | field1    | U2        | -         | -         | The       |
|           |           |           |           |           | second    |
|           |           |           |           |           | field ID  |
|           |           |           |           |           | to set    |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 7         | field1Dat | U2        | -         | -         || The      |
|           | a         |           |           |           | second    |
|           |           |           |           |           | field     |
|           |           |           |           |           | ID’s data |
|           |           |           |           |           || to set   |
+-----------+-----------+-----------+-----------+-----------+-----------+
| …         | …         | U2        | -         | -         | …         |
+-----------+-----------+-----------+-----------+-----------+-----------+
| numFields | field…    | U2        | -         | -         | The last  |
| *4        |           |           |           |           | field ID  |
| -3        |           |           |           |           | to set    |
+-----------+-----------+-----------+-----------+-----------+-----------+
| numFields | field…Dat | U2        | -         | -         | The last  |
| *4        | a         |           |           |           | field     |
| -1        |           |           |           |           | ID’s data |
|           |           |           |           |           | to set    |
+-----------+-----------+-----------+-----------+-----------+-----------+

**Set Fields Response**

+-------------+-------------+-------------+-------------+-------------+
| Set Fields  |             |             |             |             |
| (‘SF’ =     |             |             |             |             |
| 0x5346)     |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| *Preamble*  | *Packet     | *Length*    | *Payload*   | *Terminatio |
|             | Type*       |             |             | n*          |
+-------------+-------------+-------------+-------------+-------------+
| 0x5555      | 0x5346      | 1+numFields | <SF         | <CRC (U2)>  |
|             |             | *2          | payload>    |             |
+-------------+-------------+-------------+-------------+-------------+

The unit will send this packet in response to a set fields command if
the command has completed without errors.

+-----------+-----------+-----------+-----------+-----------+-----------+
| SF        |           |           |           |           |           |
| Payload   |           |           |           |           |           |
| Contents  |           |           |           |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| *Byte     | *Name*    | *Format*  | *Scaling* | *Units*   | *Descript |
| Offset*   |           |           |           |           | ion*      |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 0         | numFields | U1        | -         | -         | The       |
|           |           |           |           |           | number of |
|           |           |           |           |           | fields    |
|           |           |           |           |           | set       |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 1         | field0    | U2        | -         | -         | The first |
|           |           |           |           |           | field ID  |
|           |           |           |           |           | set       |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | field1    | U2        | -         | -         | The       |
|           |           |           |           |           | second    |
|           |           |           |           |           | field ID  |
|           |           |           |           |           | set       |
+-----------+-----------+-----------+-----------+-----------+-----------+
| …         | …         | U2        | -         | -         | More      |
|           |           |           |           |           | field IDs |
|           |           |           |           |           | set       |
+-----------+-----------+-----------+-----------+-----------+-----------+
| numFields | Field…    | U2        | -         | -         | The last  |
| *2        |           |           |           |           | field ID  |
| - 1       |           |           |           |           | set       |
+-----------+-----------+-----------+-----------+-----------+-----------+

Read Fields Command
-------------------

+-------------+-------------+-------------+-------------+-------------+
| Read Fields |             |             |             |             |
| (‘RF’ =     |             |             |             |             |
| 0x5246)     |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| *Preamble*  | *Packet     | *Length*    | *Payload*   | *Terminatio |
|             | Type*       |             |             | n*          |
+-------------+-------------+-------------+-------------+-------------+
| 0x5555      | 0x5246      | 1+numFields | <RF         | <CRC (U2)>  |
|             |             | *2          | payload>    |             |
+-------------+-------------+-------------+-------------+-------------+

This command allows the user to read the default power-up configuration
fields from the EEPROM. *NumFields* is the number of fields to read. The
*field0, field1, etc.* are the field IDs to read. RF may be used to read
configuration and calibration fields from the EEPROM. If at least one
field is successfully read, the unit will respond with a read fields
response containing the field IDs and data from the successfully read
fields. If any field is unable to be read, the unit will respond with an
error response. Note that both a read fields and an error response may
be received as a result of a read fields command.

+-----------+-----------+-----------+-----------+-----------+-----------+
| RF        |           |           |           |           |           |
| Payload   |           |           |           |           |           |
| Contents  |           |           |           |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| *Byte     | *Name*    | *Format*  | *Scaling* | *Units*   | *Descript |
| Offset*   |           |           |           |           | ion*      |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 0         | numFields | U1        | -         | -         | The       |
|           |           |           |           |           | number of |
|           |           |           |           |           | fields to |
|           |           |           |           |           | read      |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 1         | field0    | U2        | -         | -         | The first |
|           |           |           |           |           | field ID  |
|           |           |           |           |           | to read   |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | field1    | U2        | -         | -         | The       |
|           |           |           |           |           | second    |
|           |           |           |           |           | field ID  |
|           |           |           |           |           | to read   |
+-----------+-----------+-----------+-----------+-----------+-----------+
| …         | …         | U2        | -         | -         | More      |
|           |           |           |           |           | field IDs |
|           |           |           |           |           | to read   |
+-----------+-----------+-----------+-----------+-----------+-----------+
| numFields | Field…    | U2        | -         | -         | The last  |
| *2        |           |           |           |           | field ID  |
| - 1       |           |           |           |           | to read   |
+-----------+-----------+-----------+-----------+-----------+-----------+

Read Fields Response
--------------------

+-------------+-------------+-------------+-------------+-------------+
| Read Fields |             |             |             |             |
| (‘RF’ =     |             |             |             |             |
| 0x5246)     |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| *Preamble*  | *Packet     | *Length*    | *Payload*   | *Terminatio |
|             | Type*       |             |             | n*          |
+-------------+-------------+-------------+-------------+-------------+
| 0x5555      | 0x5246      | 1+numFields | <RF         | <CRC (U2)>  |
|             |             | *4          | payload>    |             |
+-------------+-------------+-------------+-------------+-------------+

The unit will send this packet in response to a read fields request if
the command has completed without errors.

+-----------+-----------+-----------+-----------+-----------+-----------+
| RF        |           |           |           |           |           |
| Payload   |           |           |           |           |           |
| Contents  |           |           |           |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| *Byte     | *Name*    | *Format*  | *Scaling* | *Units*   | *Descript |
| Offset*   |           |           |           |           | ion*      |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 0         | numFields | U1        | -         | -         | The       |
|           |           |           |           |           | number of |
|           |           |           |           |           | fields    |
|           |           |           |           |           | read      |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 1         | field0    | U2        | -         | -         | The first |
|           |           |           |           |           | field ID  |
|           |           |           |           |           | read      |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | field0Dat | U2        | -         | -         | The first |
|           | a         |           |           |           | field     |
|           |           |           |           |           | ID’s data |
|           |           |           |           |           | read      |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 5         | field1    | U2        | -         | -         | The       |
|           |           |           |           |           | second    |
|           |           |           |           |           | field ID  |
|           |           |           |           |           | read      |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 7         | field1Dat | U2        | -         | -         || The      |
|           | a         |           |           |           | second    |
|           |           |           |           |           | field     |
|           |           |           |           |           | ID’s data |
|           |           |           |           |           || read     |
+-----------+-----------+-----------+-----------+-----------+-----------+
| …         | …         | U2        | -         | -         | …         |
+-----------+-----------+-----------+-----------+-----------+-----------+
| numFields | field…    | U2        | -         | -         | The last  |
| *4        |           |           |           |           | field ID  |
| -3        |           |           |           |           | read      |
+-----------+-----------+-----------+-----------+-----------+-----------+
| numFields | field…Dat | U2        | -         | -         | The last  |
| *4        | a         |           |           |           | field     |
| -1        |           |           |           |           | ID’s data |
|           |           |           |           |           | read      |
+-----------+-----------+-----------+-----------+-----------+-----------+

Get Fields Command
------------------

+-------------+-------------+-------------+-------------+-------------+
| Get Fields  |             |             |             |             |
| (‘GF’ =     |             |             |             |             |
| 0x4746)     |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| *Preamble*  | *Packet     | *Length*    | *Payload*   | *Terminatio |
|             | Type*       |             |             | n*          |
+-------------+-------------+-------------+-------------+-------------+
| 0x5555      | 0x4746      | 1+numFields | <GF Data>   | <CRC (U2)>  |
|             |             | *2          |             |             |
+-------------+-------------+-------------+-------------+-------------+

This command allows the user to get the unit’s current configuration
fields. *NumFields* is the number of fields to get. The *field0, field1,
etc.* are the field IDs to get. GF may be used to get configuration,
calibration, and algorithm fields from RAM. Multiple algorithm fields
will not necessarily be from the same algorithm iteration. If at least
one field is successfully collected, the unit will respond with a get
fields response with data containing the field IDs of the successfully
received fields. If any field is unable to be received, the unit will
respond with an error response. Note that both a get fields and an error
response may be received as the result of a get fields command.

+-----------+-----------+-----------+-----------+-----------+--------------+
| GF        |           |           |           |           |              |
| Payload   |           |           |           |           |              |
+-----------+-----------+-----------+-----------+-----------+--------------+
| Byte      | Name      | Format    | Scaling   | Units     | Description  |
| Offset    |           |           |           |           |              |
+-----------+-----------+-----------+-----------+-----------+--------------+
| 0         | numFields | U1        | -         | -         || The         |
|           |           |           |           |           | number of    |
|           |           |           |           |           || fields to   |
|           |           |           |           |           | get          |
+-----------+-----------+-----------+-----------+-----------+--------------+
| 1         | field0    | U2        | -         | -         || The first   |
|           |           |           |           |           | field ID     |
|           |           |           |           |           || to get      |
+-----------+-----------+-----------+-----------+-----------+--------------+
| 3         | field1    | U2        | -         | -         || The         |
|           |           |           |           |           | second       |
|           |           |           |           |           || field ID    |
|           |           |           |           |           | to get       |
+-----------+-----------+-----------+-----------+-----------+--------------+
| …         | …         | U2        | -         | -         || More        |
|           |           |           |           |           | field IDs    |
|           |           |           |           |           || to get      |
+-----------+-----------+-----------+-----------+-----------+--------------+
| numFields | Field…    | U2        | -         | -         || The last    |
| *2        |           |           |           |           | field ID     |
| – 1       |           |           |           |           || to get      |
+-----------+-----------+-----------+-----------+-----------+--------------+

Get Fields Response
-------------------

+----------------------------+-------------+---------------+-----------+-------------+
| Get Fields (‘GF’ = 0x4746) |             |               |           |             |
+----------------------------+-------------+---------------+-----------+-------------+
| Preamble                   | Packet Type | Length        | Payload   | Termination |
+----------------------------+-------------+---------------+-----------+-------------+
| 0x5555                     | 0x4746      | 1+numFields*4 | <GF Data> | <CRC (U2)>  |
+----------------------------+-------------+---------------+-----------+-------------+

The unit will send this packet in response to a get fields request if
the command has completed without errors.

+-----------+-----------+-----------+-----------+-----------+-------------+
| GF        |           |           |           |           |             |
| Payload   |           |           |           |           |             |
+-----------+-----------+-----------+-----------+-----------+-------------+
| Byte      | Name      | Format    | Scaling   | Units     | Description |
| Offset    |           |           |           |           |             |
+-----------+-----------+-----------+-----------+-----------+-------------+
| 0         | numFields | U1        | -         | -         || The        |
|           |           |           |           |           | number of   |
|           |           |           |           |           || fields     |
|           |           |           |           |           | retrieved   |
+-----------+-----------+-----------+-----------+-----------+-------------+
| 1         | field0    | U2        | -         | -         || The first  |
|           |           |           |           |           | field ID    |
|           |           |           |           |           || retrieved  |
+-----------+-----------+-----------+-----------+-----------+-------------+
| 3         | field0Dat | U2        | -         | -         || The first  |
|           | a         |           |           |           | field       |
|           |           |           |           |           || ID’s data  |
|           |           |           |           |           | retrieved   |
+-----------+-----------+-----------+-----------+-----------+-------------+
| 5         | field1    | U2        | -         | -         || The        |
|           |           |           |           |           | second      |
|           |           |           |           |           || field ID   |
|           |           |           |           |           | retrieved   |
+-----------+-----------+-----------+-----------+-----------+-------------+
| 7         | field1Dat | U2        | -         | -         || The        |
|           | a         |           |           |           | second      |
|           |           |           |           |           || field      |
|           |           |           |           |           | ID’s data   |
+-----------+-----------+-----------+-----------+-----------+-------------+
| …         | …         | U2        | -         | -         | …           |
+-----------+-----------+-----------+-----------+-----------+-------------+
| numFields | field…    | U2        | -         | -         || The last   |
| *4        |           |           |           |           | field ID    |
| -3        |           |           |           |           || retrieved  |
+-----------+-----------+-----------+-----------+-----------+-------------+
| numFields | field…Dat | U2        | -         | -         || The last   |
| *4        | a         |           |           |           | field       |
| -1        |           |           |           |           || ID’s data  |
|           |           |           |           |           | retrieved   |
+-----------+-----------+-----------+-----------+-----------+-------------+


.. |image12| image:: media/image6.png
   :width: 2.53in
   :height: 1.79in
.. |image13| image:: media/image6.png
   :width: 2.53in
   :height: 1.79in
