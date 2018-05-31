DMUx81ZA Advanced UART Port BIT
*******************************

Built in Test (BIT) and Status Fields
-------------------------------------

Internal health and status are monitored and communicated in both
hardware and software. The ultimate indication of a fatal problem is a
hardware BIT signal on the user connector which is mirrored in the
software BIT field as the masterFail flag. This flag is thrown as a
result of a number of instantly fatal conditions (known as a “hard”
failure) or a persistent serious problem (known as a “soft” failure).
Soft errors are those which must be triggered multiple times within a
specified time window to be considered fatal. Soft errors are managed
using a digital high-pass error counter with a trigger threshold.

The masterStatus flag is a configurable indication as determined by the
user. This flag is asserted as a result of any asserted alert signals
which the user has enabled.

The hierarchy of BIT and Status *fields* and signals is depicted here:

**BITstatus Field**

   masterFail

      hardwareError

         hardwareBIT Field

            powerError

               hardwarePowerBIT Field

                  inpPower

                  inpCurrent

                  inpVoltage

                  fiveVolt

                  threeVolt

                  twoVolt

                  twoFiveRef

                  sixVolt

                  grdRef

          environmentalError

             hardwareEnvironmentalBIT Field

                pcbTemp

        comError

           comBIT Field

              serialAError

                 comSerialABIT Field

                    transmitBufferOverflow

                    receiveBufferOverflow

                    framingError

                    breakDetect

                    parityError

              serialBError

                 comSerialBBIT Field

                    transmitBufferOverflow

                    receiveBufferOverflow

                    framingError

                    breakDetect

                    parityError

         softwareError

            softwareBIT Field 

               algorithmError

                  softwareAlgorithmBIT Field

                     initialization

                     overRange

                     missedIntegrationStep

               dataError

                  softwareDataBIT Field

                     calibrationCRCError

                     magAlignOutOfBounds

      masterStatus

         hardwareStatus

            hardwareStatus Field

               unlocked1PPS (enabled by default on INS)

               unlockedInternalGPS (enabled by default on INS)

               noDGPS

               unlockedEEPROM

         comStatus

            comStatus Field

               noExternalGPS (enabled by default on VG and AHRS)

         softwareStatus

            softwareStatus Field

               algorithmInitialization (enabled by default)

               highGain (enabled by default)

               attitudeOnlyAlgorithm

               turnSwitch

          sensorStatus

             sensorStatus Field

                overRange (enabled by default)


Master BIT and Status (BITstatus) Field
---------------------------------------

The BITstatus field is the global indication of health and status of the
DMUx81ZA Series product (See Table 47). The LSB contains BIT information
and the MSB contains status information.

There are four intermediate signals that are used to determine when
masterFail and the hardware BIT signal are asserted. These signals are
controlled by various systems checks in software that are classified
into three categories: hardware, communication, and software.
Instantaneous soft failures in each of these four categories will
trigger these intermediate signals, but will not trigger the masterFail
until the persistency conditions are met.

There are four intermediate signals that are used to determine when the
masterStatus flag is asserted: hardwareStatus, sensorStatus, comStatus,
and softwareStatus. masterStatus is the logical OR of these intermediate
signals. Each of these intermediate signals has a separate field with
individual indication flags. Each of these indication flags can be
enabled or disabled by the user. Any enabled indication flag will
trigger the associated intermediate signal and masterStatus flag.

                **Table 47 DMUx81 BIT Status Field**

+-----------------+-----------------+-----------------+-----------------+
| **BITstatus     | **Bits**        | **Meaning**     | **Category**    |
| Field**         |                 |                 |                 |
+-----------------+-----------------+-----------------+-----------------+
| masterFail      | 0               | 0 = normal, 1 = | BIT             |
|                 |                 | fatal error has |                 |
|                 |                 | occurred        |                 |
+-----------------+-----------------+-----------------+-----------------+
| HardwareError   | 1               | 0 = normal, 1=  | BIT             |
|                 |                 | internal        |                 |
|                 |                 | hardware error  |                 |
+-----------------+-----------------+-----------------+-----------------+
| comError        | 2               | 0 = normal, 1 = | BIT             |
|                 |                 | communication   |                 |
|                 |                 | error           |                 |
+-----------------+-----------------+-----------------+-----------------+
| softwareError   | 3               | 0 = normal, 1 = | BIT             |
|                 |                 | internal        |                 |
|                 |                 | software error  |                 |
+-----------------+-----------------+-----------------+-----------------+
| Reserved        | 4:7             | N/A             |                 |
+-----------------+-----------------+-----------------+-----------------+
| masterStatus    | 8               | 0 = nominal, 1  | Status          |
|                 |                 | = hardware,     |                 |
|                 |                 | sensor, com, or |                 |
|                 |                 | software alert  |                 |
+-----------------+-----------------+-----------------+-----------------+
| hardwareStatus  | 9               | 0 = nominal, 1  | Status          |
|                 |                 | = programmable  |                 |
|                 |                 | alert           |                 |
+-----------------+-----------------+-----------------+-----------------+
| comStatus       | 10              | 0 = nominal, 1  | Status          |
|                 |                 | = programmable  |                 |
|                 |                 | alert           |                 |
+-----------------+-----------------+-----------------+-----------------+
| softwareStatus  | 11              | 0 = nominal, 1  | Status          |
|                 |                 | = programmable  |                 |
|                 |                 | alert           |                 |
+-----------------+-----------------+-----------------+-----------------+
| sensorStatus    | 12              | 0 = nominal, 1  | Status          |
|                 |                 | = programmable  |                 |
|                 |                 | alert           |                 |
+-----------------+-----------------+-----------------+-----------------+
| Reserved        | 13:15           | N/A             |                 |
+-----------------+-----------------+-----------------+-----------------+

HardwareBIT Field
-----------------

The hardwareBIT field contains flags that indicate various types of
internal hardware errors (See Table 48). Each of these types has an
associated message with low level error signals. The hardwareError flag
in the BITstatus field is the bit-wise OR of this hardwareBIT field.

            **Table 48 DMUx81 Hardware BIT Field**

+-------------------------+------------+-----------------------+----------------+
| **hardwareBIT Field**   | **Bits**   | **Meaning**           | **Category**   |
+-------------------------+------------+-----------------------+----------------+
| powerError              | 0          | 0 = normal, 1 = error | Soft           |
+-------------------------+------------+-----------------------+----------------+
| environmentalError      | 1          | 0 = normal, 1 = error | Soft           |
+-------------------------+------------+-----------------------+----------------+
| reserved                | 2:15       | N/A                   |                |
+-------------------------+------------+-----------------------+----------------+

HardwarePowerBIT Field
----------------------

The hardwarePowerBIT field contains flags that indicate low level power
system errors (See Table 49). The powerError flag in the hardwareBIT
field is the bit-wise OR of this hardwarePowerBIT field.

             **Table 49 DMUx81 Hardware Power BIT Field**

+-----------------+-----------------+-----------------+-----------------+
| **hardwarePowe  | **Bits**        | **Meaning**     | **Category**    |
| rBIT            |                 |                 |                 |
| Field**         |                 |                 |                 |
+-----------------+-----------------+-----------------+-----------------+
| inpPower        | 0               | 0 = normal, 1 = | Soft            |
|                 |                 | out of bounds   |                 |
+-----------------+-----------------+-----------------+-----------------+
| inpCurrent      | 1               | 0 = normal, 1 = | Soft            |
|                 |                 | out of bounds   |                 |
+-----------------+-----------------+-----------------+-----------------+
| inpVoltage      | 2               | 0 = normal, 1 = | Soft            |
|                 |                 | out of bounds   |                 |
+-----------------+-----------------+-----------------+-----------------+
| fiveVolt        | 3               | 0 = normal, 1 = | Soft            |
|                 |                 | out of bounds   |                 |
+-----------------+-----------------+-----------------+-----------------+
| threeVolt       | 4               | 0 = normal, 1 = | Soft            |
|                 |                 | out of bounds   |                 |
+-----------------+-----------------+-----------------+-----------------+
| twoVolt         | 5               | 0 = normal, 1 = | Soft            |
|                 |                 | out of bounds   |                 |
+-----------------+-----------------+-----------------+-----------------+
| twoFiveRef      | 6               | 0 = normal, 1 = | Soft            |
|                 |                 | out of bounds   |                 |
+-----------------+-----------------+-----------------+-----------------+
| sixVolt         | 7               | 0 = normal, 1 = | Soft            |
|                 |                 | out of bounds   |                 |
+-----------------+-----------------+-----------------+-----------------+
| grdRef          | 8               | 0 = normal, 1 = | Soft            |
|                 |                 | out of bounds   |                 |
+-----------------+-----------------+-----------------+-----------------+
| Reserved        | 9:15            | N/A             |                 |
+-----------------+-----------------+-----------------+-----------------+

HardwareEnvironmentalBIT Field
------------------------------

The hardwareEnvironmentalBIT field contains flags that indicate low
level hardware environmental errors (See Table 50). The
environmentalError flag in the hardwareBIT field is the bit-wise OR of
this hardwareEnvironmentalBIT field.

         **Table 50 DMUx81 Hardware Environment BIT Field**

+-----------------+-----------------+-----------------+-----------------+
| **hardwareEnvi  | **Bits**        | **Meaning**     | **Category**    |
| ronmentalBIT    |                 |                 |                 |
| Field**         |                 |                 |                 |
+-----------------+-----------------+-----------------+-----------------+
| pcbTemp         | 0               | 0 = normal, 1 = | Soft            |
|                 |                 | out of bounds   |                 |
+-----------------+-----------------+-----------------+-----------------+
| Reserved        | 9:15            | N/A             |                 |
+-----------------+-----------------+-----------------+-----------------+

comBIT Field
------------

The comBIT field contains flags that indicate communication errors with
external devices (See Table 51). Each external device has an associated
message with low level error signals. The comError flag in the BITstatus
field is the bit-wise OR of this comBIT field.

           **Table 51 DMUx81 COM BIT Field**

+--------------------+------------+-----------------------+----------------+
| **comBIT Field**   | **Bits**   | **Meaning**           | **Category**   |
+--------------------+------------+-----------------------+----------------+
| serialAError       | 0          | 0 = normal, 1 = error | Soft           |
+--------------------+------------+-----------------------+----------------+
| serialBError       | 1          | 0 = normal, 1 = error | Soft           |
+--------------------+------------+-----------------------+----------------+
| Reserved           | 2:15       | N/A                   |                |
+--------------------+------------+-----------------------+----------------+

comSerialABIT Field
-------------------

The comSerialABIT field (See Table 52) contains flags that indicate low
level errors with external serial port A (the user serial port). The
serialAError flag in the comBIT field is the bit-wise OR of this
comSerialABIT field.

            **Table 52 DMUx81 Serial Port A BIT Field**

+-----------------+-----------------+-----------------+-----------------+
| **comSerialABI  | **Bits**        | **Meaning**     | **Category**    |
| T               |                 |                 |                 |
| Field**         |                 |                 |                 |
+-----------------+-----------------+-----------------+-----------------+
| transmitBufferO | 0               | 0 = normal, 1 = | Soft            |
| verflow         |                 | overflow        |                 |
+-----------------+-----------------+-----------------+-----------------+
| receiveBufferOv | 1               | 0 = normal, 1 = | Soft            |
| erflow          |                 | overflow        |                 |
+-----------------+-----------------+-----------------+-----------------+
| framingError    | 2               | 0 = normal, 1 = | Soft            |
|                 |                 | error           |                 |
+-----------------+-----------------+-----------------+-----------------+
| breakDetect     | 3               | 0 = normal, 1 = | Soft            |
|                 |                 | error           |                 |
+-----------------+-----------------+-----------------+-----------------+
| parityError     | 4               | 0 = normal, 1 = | Soft            |
|                 |                 | error           |                 |
+-----------------+-----------------+-----------------+-----------------+
| Reserved        | 5:15            | N/A             |                 |
+-----------------+-----------------+-----------------+-----------------+

comSerialBBIT Field
-------------------

The comSerialBBIT field (See Table 53) contains flags that indicate low
level errors with external serial port B (the aiding serial port). The
serialBError flag in the comBIT field is the bit-wise OR of this
comSerialBBIT field.

             **Table 53 DMUx81 Serial Port B BIT Field**

+-----------------+-----------------+-----------------+-----------------+
| **comSerialBBI  | **Bits**        | **Meaning**     | **Category**    |
| T               |                 |                 |                 |
| Field**         |                 |                 |                 |
+-----------------+-----------------+-----------------+-----------------+
| transmitBufferO | 0               | 0 = normal, 1 = | Soft            |
| verflow         |                 | overflow        |                 |
+-----------------+-----------------+-----------------+-----------------+
| receiveBufferOv | 1               | 0 = normal, 1 = | Soft            |
| erflow          |                 | overflow        |                 |
+-----------------+-----------------+-----------------+-----------------+
| framingError    | 2               | 0 = normal, 1 = | Soft            |
|                 |                 | error           |                 |
+-----------------+-----------------+-----------------+-----------------+
| breakDetect     | 3               | 0 = normal, 1 = | Soft            |
|                 |                 | error           |                 |
+-----------------+-----------------+-----------------+-----------------+
| parityError     | 4               | 0 = normal, 1 = | Soft            |
|                 |                 | error           |                 |
+-----------------+-----------------+-----------------+-----------------+
| Reserved        | 5:15            | N/A             |                 |
+-----------------+-----------------+-----------------+-----------------+

SoftwareBIT Field
-----------------

The softwareBIT field contains flags that indicate various types of
software errors (See Table 54). Each type has an associated message with
low level error signals. The softwareError flag in the BITstatus field
is the bit-wise OR of this softwareBIT field.

              **Table 54 DMUx81 Softrware BIT Field**

+-------------------------+------------+-----------------------+----------------+
| **softwareBIT Field**   | **Bits**   | **Meaning**           | **Category**   |
+-------------------------+------------+-----------------------+----------------+
| algorithmError          | 0          | 0 = normal, 1 = error | Soft           |
+-------------------------+------------+-----------------------+----------------+
| dataError               | 1          | 0 = normal, 1 = error | Soft           |
+-------------------------+------------+-----------------------+----------------+
| Reserved                | 2:15       | N/A                   |                |
+-------------------------+------------+-----------------------+----------------+

SoftwareAlgorithmBIT Field
--------------------------

The softwareAlgorithmBIT field contains flags that indicate low level
software algorithm errors (See Table 55). The algorithmError flag in the
softwareBIT field is the bit-wise OR of this softwareAlgorithmBIT field.

               **Table 55 DMUx81 Software Algorithm BIT Field**

+-----------------+-----------------+-----------------+-----------------+
|| SoftwareAlgo   |   Bits          |   Meaning       |   Category      |
|| BIT Field      |                 |                 |                 |
+-----------------+-----------------+-----------------+-----------------+
| initialization  | 0               || 0 = normal, 1 =| Hard            |
|                 |                 | error during    |                 |
|                 |                 || algorithm      |                 |
|                 |                 | initialization  |                 |
+-----------------+-----------------+-----------------+-----------------+
| overRange       | 1               || 0 = normal, 1 =| Hard            |
|                 |                 | fatal sensor    |                 |
|                 |                 || over-range     |                 |
+-----------------+-----------------+-----------------+-----------------+
| missedNavigatio | 2               || 0 = normal, 1 =| Hard            |
| nStep           |                 | fatal hard      |                 |
|                 |                 || deadline missed|                 |
|                 |                 | for navigation  |                 |
+-----------------+-----------------+-----------------+-----------------+
| Reserved        | 3:15            | N/A             |                 |
+-----------------+-----------------+-----------------+-----------------+

SoftwareDataBIT Field
---------------------

The softwareDataBIT field contains flags that indicate low level
software data errors (See Table 56). The dataError flag in the
softwareBIT field is the bit-wise OR of this softwareDataBIT field.

               **Table 56 DMUx81 Software Data BIT Field**

+-----------------+-----------------+-----------------+-----------------+
|| SoftwareData   |   Bits          |   Meaning       |   Category      |
|| BIT Field      |                 |                 |                 |
+-----------------+-----------------+-----------------+-----------------+
|| calibration    | 0               || 0 = normal, 1 =| Hard            |
|| CRC Error      |                 | incorrect CRC   |                 |
|                 |                 || on calibration |                 |
|                 |                 | EEPROM data or  |                 |
|                 |                 || data has been  |                 |
|                 |                 | compromised by  |                 |
|                 |                 || a WE command.  |                 |
+-----------------+-----------------+-----------------+-----------------+
|| magAlign       | 1               || 0 = normal, 1 =| Hard            |
|| Out of Bounds  |                 | hard and soft   |                 |
|                 |                 || iron parameters|                 |
|                 |                 | are out of      |                 |
|                 |                 || bounds         |                 |
+-----------------+-----------------+-----------------+-----------------+
| Reserved        | 2:15            | N/A             |                 |
+-----------------+-----------------+-----------------+-----------------+

HardwareStatus Field
--------------------

The hardwareStatus field contains flags that indicate various internal
hardware conditions and alerts that are not errors or problems (See
Table 57). The hardwareStatus flag in the BITstatus field is the
bit-wise OR of the logical AND of the hardwareStatus field and the
hardwareStatusEnable field. The hardwareStatusEnable field is a bit mask
that allows the user to select items of interest that will logically
flow up to the masterStatus flag.

                **Table 57 DMUx81 Hardware Status BIT Field**

+-----------------------+-----------------------+-----------------------+
| **hardwareStatus      | **Bits**              | **Meaning**           |
| Field**               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| unlocked1PPS          | 0                     | 0 = not asserted, 1 = |
|                       |                       | asserted              |
+-----------------------+-----------------------+-----------------------+
| unlockedInternalGPS   | 1                     | 0 = not asserted, 1 = |
|                       |                       | asserted              |
+-----------------------+-----------------------+-----------------------+
| noDGPS                | 2                     | 0 = DGPS lock, 1 = no |
|                       |                       | DGPS                  |
+-----------------------+-----------------------+-----------------------+
| unlockedEEPROM        | 3                     | 0=locked, WE          |
|                       |                       | disabled, 1=unlocked, |
|                       |                       | WE enabled            |
+-----------------------+-----------------------+-----------------------+
| Reserved              | 4:15                  | N/A                   |
+-----------------------+-----------------------+-----------------------+

comStatus Field
---------------

The comStatus field contains flags that indicate various external
communication conditions and alerts that are not errors or problems (See
Table 58). The comStatus flag in the BITstatus field is the bit-wise OR
of the logical AND of the comStatus field and the comStatusEnable field.
The comStatusEnable field is a bit mask that allows the user to select
items of interest that will logically flow up to the masterStatus flag.

               **Table 58 DMUx81 COM Status BIT Field**

+-----------------------+-----------------------+-----------------------+
| **comStatus Field**   | **Bits**              | **Meaning**           |
+-----------------------+-----------------------+-----------------------+
| noExternalGPS         | 0                     || 0 = external GPS data|
|                       |                       | is being received, 1  |
|                       |                       || = no external GPS    |
|                       |                       | data is available     |
+-----------------------+-----------------------+-----------------------+
| Reserved              | 1:15                  | N/A                   |
+-----------------------+-----------------------+-----------------------+

softwareStatus Field
--------------------

The softwareStatus field contains flags that indicate various software
conditions and alerts that are not errors or problems (See Table 59).
The softwareStatus flag in the BITstatus field is the bit-wise OR of the
logical AND of the softwareStatus field and the softwareStatusEnable
field. The softwareStatusEnable field is a bit mask that allows the user
to select items of interest that will logically flow up to the
masterStatus flag.

                **Table 59 DMUx81 Software Status Field**

+-----------------------+-----------------------+-----------------------+
| **softwareStatus      | **Bits**              | **Meaning**           |
| Field**               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| algorithmInit         | 0                     | 0 = normal, 1 = the   |
|                       |                       | algorithm is in       |
|                       |                       | initialization mode   |
+-----------------------+-----------------------+-----------------------+
| highGain              | 1                     | 0 = low gain mode, 1  |
|                       |                       | high gain mode        |
+-----------------------+-----------------------+-----------------------+
| attitudeOnlyAlgorithm | 2                     | 0 = navigation state  |
|                       |                       | tracking, 1 =         |
|                       |                       | attitude only state   |
|                       |                       | tracking              |
+-----------------------+-----------------------+-----------------------+
| turnSwitch            | 3                     | 0 = off, 1 = yaw rate |
|                       |                       | greater than          |
|                       |                       | turnSwitch threshold  |
+-----------------------+-----------------------+-----------------------+
| Reserved              | 4:15                  | N/A                   |
+-----------------------+-----------------------+-----------------------+

sensorStatus Field
------------------

The sensorStatus field contains flags that indicate various internal
sensor conditions and alerts that are not errors or problems (See Table
61). The sensorStatus flag in the BITstatus field is the bit-wise OR of
the logical AND of the sensorStatus field and the sensorStatusEnable
field. The sensorStatusEnable field is a bit mask that allows the user
to select items of interest that will logically flow up to the
masterStatus flag.

                **Table 60 DMUx81 Sensor Status Field**

+--------------------------+------------+--------------------------------+
| **sensorStatus Field**   | **Bits**   | **Meaning**                    |
+--------------------------+------------+--------------------------------+
| overRange                | 0          | 0 = not asserted, 1 = asserted |
+--------------------------+------------+--------------------------------+
| Reserved                 | 1:15       | N/A                            |
+--------------------------+------------+--------------------------------+

Configuring the Master Status
-----------------------------

The masterStatus byte and its associated programmable alerts are
configured using the Read Field and Write Field command as described in
Section `8 <\l>`__, Advanced Commands. Table 61 shows the definition of
the bit mask for configuring the status signals.

            **Table 61 DMUx81 Master Status Byte Configuration Fields**

+-----------------+-----------------+-----------------+-----------------+
| **configuration | **field ID**    | **Valid         | **Description** |
| fields**        |                 | Values**        |                 |
+-----------------+-----------------+-----------------+-----------------+
| hardwareStatusE | 0x0010          | Any             || Bit mask of    |
| nable           |                 |                 | enabled         |
|                 |                 |                 || hardware status|
|                 |                 |                 | signals         |
+-----------------+-----------------+-----------------+-----------------+
| comStatusEnable | 0x0011          | Any             || Bit mask of    |
|                 |                 |                 | enabled         |
|                 |                 |                 || communication  |
|                 |                 |                 | status signals  |
+-----------------+-----------------+-----------------+-----------------+
| softwareStatusE | 0x0012          | Any             || Bit mask of    |
| nable           |                 |                 | enabled         |
|                 |                 |                 || software status|
|                 |                 |                 | signals         |
+-----------------+-----------------+-----------------+-----------------+
| sensorStatusEna | 0x0013          | Any             || Bit mask of    |
| ble             |                 |                 | enabled sensor  |
|                 |                 |                 || status signals |
+-----------------+-----------------+-----------------+-----------------+

hardwareStatusEnable Field
--------------------------

This field is a bit mask of the hardwareStatus field (see BIT and status
definitions). This field allows the user to determine which low level
hardwareStatus field signals will flag the hardwareStatus and
masterStatus flags in the BITstatus field. Any asserted bits in this
field imply that the corresponding hardwareStatus field signal, if
asserted, will cause the hardwareStatus and masterStatus flags to be
asserted in the BITstatus field.

comStatusEnable Field
---------------------

This field is a bit mask of the comStatus field (see BIT and status
definitions). This field allows the user to determine which low level
comStatus field signals will flag the comStatus and masterStatus flags
in the BITstatus field. Any asserted bits in this field imply that the
corresponding comStatus field signal, if asserted, will cause the
comStatus and masterStatus flags to be asserted in the BITstatus field.

softwareStatusEnable Field
--------------------------

This field is a bit mask of the softwareStatus field (see BIT and status
definitions). This field allows the user to determine which low level
softwareStatus field signals will flag the softwareStatus and
masterStatus flags in the BITstatus field. Any asserted bits in this
field imply that the corresponding softwareStatus field signal, if
asserted, will cause the softwareStatus and masterStatus flags to be
asserted in the BITstatus field.

sensorStatusEnable Field
------------------------

This field is a bit mask of the sensorStatus field (see BIT and status
definitions). This field allows the user to determine which low level
sensorStatus field signals will flag the sensorStatus and masterStatus
flags in the BITstatus field. Any asserted bits in this field imply that
the corresponding sensorStatus field signal, if asserted, will cause the
sensorStatus and masterStatus flags to be asserted in the BITstatus
field.
