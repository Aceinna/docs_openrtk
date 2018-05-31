DMUx81 UART Port Interface Definition
*************************************

The DMUx81ZA Series contains a number of different products which have
different measurement capabilities. Depending on the model you
purchased, various commands and output modes are supported. However, all
models support a common packet structure that includes both command or
input data packets (data sent to the DMUx81ZA Series) and measurement
output or response packet formats (data sent from the DMUx81ZA Series).
This section of the manual explains these packet formats as well as the
supported commands. NAV-VIEW also features a number of tools that can
help a user understand the packet types available and the information
contained within the packets. This section of the manual assumes that
the user is familiar with ANSI C programming language and data type
conventions.

For an example of the code required to parse input data packets, please
see refer to Appendix C.

For qualified commercial OEM users, a source code license of NAV-VIEW
can be made available under certain conditions. Please contact your
ACEINNA representative for more information.

General Settings
----------------

The serial port settings are RS232 with 1 start bit, 8 data bits, no
parity bit, 1 stop bit, and no flow control. Standard baud rates
supported are: 38400, 57600, 115200, and 230400.

Common definitions include:

A word is defined to be 2 bytes or 16 bits.

All communications to and from the unit are packets that start with a
single word alternating bit preamble 0x5555. This is the ASCII string
“UU”.

All multiple byte values are transmitted Big Endian (Most Significant
Byte First).

All communication packets end with a single word CRC (2 bytes). CRC’s
are calculated on all packet bytes excluding the preamble and CRC
itself. Input packets with incorrect CRC’s will be ignored.

Each complete communication packet must be transmitted to the DMUx81ZA
Series inertial system within a 4 second period.

Number Formats
--------------

Number Format Conventions include:

0x as a prefix to hexadecimal values

single quotes (‘’) to delimit ASCII characters

no prefix or delimiters to specify decimal values.

Table 38 defines number formats:

              **Table 38 Number Formats**

+-------------+-------------+-------------+-------------+-------------+
| **Descripto | **Descripti | **Size      | **Comment** | **Range**   |
| r**         | on**        | (bytes)**   |             |             |
+-------------+-------------+-------------+-------------+-------------+
| U1          | Unsigned    | 1           |             | 0 to 255    |
|             | Char        |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| U2          | Unsigned    | 2           |             | 0 to 65535  |
|             | Short       |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| U4          | Unsigned    | 4           |             | 0 to 2^32-1 |
|             | Int         |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| I2          | Signed      | 2           | 2’s         | -2^15 to    |
|             | Short       |             | Complement  | 2^15-1      |
+-------------+-------------+-------------+-------------+-------------+
| I2\*        | Signed      | 2           | Shifted 2’s | Shifted to  |
|             | Short       |             | Complement  | specified   |
|             |             |             |             | range       |
+-------------+-------------+-------------+-------------+-------------+
| I4          | Signed Int  | 4           | 2’s         | -2^31 to    |
|             |             |             | Complement  | 2^31-1      |
+-------------+-------------+-------------+-------------+-------------+
| F4          | Floating    | 4           | IEEE754     | -1*2^127 to |
|             | Point       |             | Single      | 2^127       |
|             |             |             | Precision   |             |
+-------------+-------------+-------------+-------------+-------------+
| SN          | String      | N           | ASCII       |             |
+-------------+-------------+-------------+-------------+-------------+

Packet Format
-------------

All of the Input and Output packets, except the Ping command, conform to
the following structure:

+-------------+-------------+-------------+-------------+-------------+
| 0x5555      || <2-byte    || <payload   || <variable  ||  <2-byte   |
|             || packet type|| byte-length|| length     || CRC (U2)>  |
|             || (U2)>      || (U1)>      || payload>   ||            |
+-------------+-------------+-------------+-------------+-------------+

The Ping Command does not require a CRC, so a DMUx81ZA Series unit can
be pinged from a terminal emulator. To Ping a DMUx81ZA Series unit, type
the ASCII string ‘UUPK’. If properly connected, the DMUx81ZA Series unit
will respond with ‘PK’. All other communications with the DMUx81ZA
Series unit require the 2-byte CRC. {Note: A DMUx81ZA Series unit will
also respond to a ping command using the full packet formation with
payload 0 and correctly calculated CRC. Example: 0x5555504B009ef4 }.

Packet Header
-------------

The packet header is always the bit pattern 0x5555.

Packet Type
-----------


The packet type is always two bytes long in unsigned short integer
format. Most input and output packet types can be interpreted as a pair
of ASCII characters. As a semantic aid consider the following single
character acronyms:

    P = packet

    F = fields

    Refers to Fields which are settings or data contained in the unit

    E = EEPROM

    Refers to factory data stored in EEPROM

    R = read

    Reads default non-volatile fields

    G = get

    Gets current volatile fields or settings

    W = write

    Writes default non-volatile fields. These fields are stored in
    non-volatile memory and determine the unit’s behavior on power up.
    Modifying default fields take effect on the next power up and
    thereafter.

    S = set

    Sets current volatile fields or settings. Modifying current fields
    will take effect immediately by modifying internal RAM and are lost
    on a power cycle.

Payload Length
--------------

The payload length is always a one byte unsigned character with a range
of 0-255. The payload length byte is the length (in bytes) of the
*<variable length payload>* portion of the packet ONLY, and does not
include the CRC.

Payload
-------

The payload is of variable length based on the packet type.

16-bit CRC-CCITT
----------------

Packets end with a 16-bit CRC-CCITT calculated on the entire packet
excluding the 0x5555 header and the CRC field itself. A discussion of
the 16-bit CRC-CCITT and sample code for implementing the computation of
the CRC is included at the end of this document. This 16-bit CRC
standard is maintained by the International Telecommunication Union
(ITU). The highlights are:

Width = 16 bits

Polynomial 0x1021

Initial value = 0xFFFF

No XOR performed on the final value.

See Appendix C for sample code that implements the 16-bit CRC algorithm.

Messaging Overview
------------------

Table 39 summarizes the messages available by DMUx81ZA Series model.
Packet types are assigned mostly using the ASCII mnemonics defined above
and are indicated in the summary table below and in the detailed
sections for each command. The payload byte-length is often related to
other data elements in the packet as defined in the table below. The
referenced variables are defined in the detailed sections following.
Output messages are sent from the DMUx81ZA Series inertial system to the
user system as a result of a poll request or a continuous packet output
setting. Input messages are sent from the user system to the DMUx81ZA
Series inertial system and will result in an associated Reply Message or
NAK message. Note that reply messages typically have the same **<2-byte
packet type (U2)>** as the input message that evoked it but with a
different payload.

                 **Table 39 Message Table**

+---------------+-----------+-------------+-------------+-------------+-----------+
| ASCII         || <2-byte  || <payload   | Description | Type        | Products  |
|               || packet   || byte length|             |             | Available |
|               || type     || (U1)>      |             |             |           |
|               || (U2)>    ||            |             |             |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| Link Test     |           |             |             |             |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| PK            | 0x504B    | 0           || Ping       || Input/Reply| ALL       |
|               |           |             || Command    || Message    |           |
|               |           |             || and        ||            |           |
|               |           |             || Response   ||            |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| CH            | 0x4348    | N           || Echo       || Input/Reply| ALL       |
|               |           |             || Command    || Message    |           |
|               |           |             || and        ||            |           |
|               |           |             || Response   ||            |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
|| Interactive  |           |             |             |             |           |
|| Commands     |           |             |             |             |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| GP            | 0x4750    | 2           || Get        || Input      | ALL       |
|               |           |             || Packet     || Message    |           |
|               |           |             || Request    ||            |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| AR            | 0x4152    | 0           || Algorithm  || Input/Reply|| VG,AHRS, |
|               |           |             || Reset      || Message    || INS      |
|               |           |             ||            ||            ||          |
+---------------+-----------+-------------+-------------+-------------+-----------+
| NAK           | 0x1515    | 2           || Error      || Reply      | ALL       |
|               |           |             || Response   || Message    |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| WC            | 0x5743    | 2           || Calibrate  || Input/Reply| AHRS, INS |
|               |           |             || Command    || Message    |           |
|               |           |             || and        ||            |           |
|               |           |             || Response   ||            |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| CD            | 0x4344    | 10          || Calibrati  || Reply      | AHRS, INS |
|               |           |             || on         || Message    |           |
|               |           |             || Completed  ||            |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
|| Output       |           |             |             |             |           |
|| Messages:    |           |             |             |             |           |
|| Status &     |           |             |             |             |           |
|| Other(Polled |           |             |             |             |           |
|| Only)        |           |             |             |             |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| ID            | 0x4944    | 5+N         || ID         || Output     | ALL       |
|               |           |             || Data       || Message    |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| VR            | 0x5652    | 5           || Version    || Output     | ALL       |
|               |           |             || Data       || Message    |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| T0            | 0x5430    | 28          || Test 0     || Output     | ALL       |
|               |           |             || (Detailed  || Message    |           |
|               |           |             || BIT and    ||            |           |
|               |           |             || Status)    ||            |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
|| Output       |           |             |             |             |           |
|| Messages:    |           |             |             |             |           |
|| Measurement  |           |             |             |             |           |
|| Data         |           |             |             |             |           |
|| (Continuous  |           |             |             |             |           |
|| or Polled)   |           |             |             |             |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| S0            | 0x5330    | 30          || Scaled     || Output     || IMUx81ZA |
|               |           |             || Sensor 0   || Message    ||          |
|               |           |             || Data       ||            ||          |
|               |           |             ||            ||            || AHRS, INS|
+---------------+-----------+-------------+-------------+-------------+-----------+
| S1            | 0x5331    | 24          || Scaled     || Output     | ALL       |
|               |           |             || Sensor 1   || Message    |           |
|               |           |             || Data       ||            |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| A1            | 0x4131    | 32          || Angle 1    || Output     | AHRS, INS |
|               |           |             || Data       || Message    |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| A2            | 0x4132    | 30          || Angle 2    || Output     | VG, AHRS, |
|               |           |             || Data       || Message    | INS       |
+---------------+-----------+-------------+-------------+-------------+-----------+
| A3            | 0x4133    | 30          || Angle 3    || Output     | VG, AHRS, |
|               |           |             || Data       || Message    | INS       |
+---------------+-----------+-------------+-------------+-------------+-----------+
| N0            | 0x4E30    | 32          || Nav 0      || Output     | VG, AHRS, |
|               |           |             || Data       || Message    | INS       |
+---------------+-----------+-------------+-------------+-------------+-----------+
| N1            | 0x4E31    | 42          || Nav 1      || Output     | VG, AHRS, |
|               |           |             || Data       || Message    | INS       |
+---------------+-----------+-------------+-------------+-------------+-----------+
|| Advanced     |           |             |             |             |           |
|| Commands     |           |             |             |             |           | 
+---------------+-----------+-------------+-------------+-------------+-----------+
| WF            | 0x5746    | numFields   || Write      || Input      | ALL       |
|               |           | *4+1        || Fields     || Message    |           |
|               |           |             || Request    ||            |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| WF            | 0x5746    | numFields   || Write      || Reply      | ALL       |
|               |           | *2+1        || Fields     || Message    |           |
|               |           |             || Response   ||            |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| SF            | 0x5346    | numFields   || Set        || Input      | ALL       |
|               |           | *4+1        || Fields     || Message    |           |
|               |           |             || Request    ||            |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| SF            | 0x5346    | numFields   || Set        || Reply      | ALL       |
|               |           | *2+1        || Fields     ||            |           |
|               |           |             || Response   || Message    |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| RF            | 0x5246    | numFields   || Read       || Input      | ALL       |
|               |           | *2+1        || Fields     || Message    |           |
|               |           |             || Request    ||            |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| RF            | 0x5246    | numFields   || Read       || Reply      | ALL       |
|               |           | *4+1        || Fields     || Message    |           |
|               |           |             || Response   ||            |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| GF            | 0x4746    | numFields   || Get        || Input      | ALL       |
|               |           | *2+1        || Fields     || Message    |           |
|               |           |             || Request    ||            |           |
+---------------+-----------+-------------+-------------+-------------+-----------+
| GF            | 0x4746    | numFields   || Get        || Reply      | ALL       |
|               |           | *4+1        || Fields     || 1Message   |           |
|               |           |             || Response   ||            |           |
+---------------+-----------+-------------+---------------------------+-----------+

