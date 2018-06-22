Messaging Modules
=================

.. contents:: Contents
    :local:

OpenIMU UART messaging framework
*************************

1. General Settings
----------------

The serial port settings are: 1 start bit, 8 data bits, no
parity bit, 1 stop bit, and no flow control. Standard baud rates
supported are: 38400, 57600, 115200, 230400 and 460800.

Common definitions include:

A word is defined to be 2 bytes or 16 bits.

All communications to and from the unit are packets that start with a
single word alternating bit preamble 0x5555. This is the ASCII string
“UU”.


All communication packets end with a single word CRC (2 bytes). CRC’s
are calculated on all packet bytes excluding the preamble and CRC
itself. Input packets with incorrect CRC’s will be ignored.

All multiple byte values except CRC and packet code are transmitted in Little Endian format
(Least Significant Byte First).

Each complete communication packet must be transmitted to the OpenIMU300xx
inertial system within a 4 second period.

2. Number Formats
--------------

Number Format Conventions include:

0x as a prefix to hexadecimal values

single quotes (‘’) to delimit ASCII characters

no prefix or delimiters to specify decimal values.

Table below defines number formats:


+-------------+-------------+-------------+-------------+-------------+
| **ID**      | **Type**    |  **Size**   | **Comment** | **Range**   |
|             |             |  (bytes)    |             |             |
+-------------+-------------+-------------+-------------+-------------+
| U1          | Unsigned    | 1           |             | 0 to 255    |
|             | Char        |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| U2          | Unsigned    | 2           | Transmitted | 0 to 65535  |
|             | Short       |             | in Little   |             |
|             |             |             |Endian Format|             |
+-------------+-------------+-------------+-------------+-------------+
| U4          | Unsigned    | 4           | Transmitted | 0 to 2^32-1 |
|             | Int         |             | in Little   |             |
|             |             |             |Endian Format|             |
+-------------+-------------+-------------+-------------+-------------+
| U8          | Unsigned    | 8           | Transmitted | 0 to 2^64-1 |
|             | long long   |             | in Little   |             |
|             |             |             |Endian Format|             |
+-------------+-------------+-------------+-------------+-------------+
| F4          | Float       | 4           | Transmitted |             |
|             |             |             | in Little   |             |
|             | IEEE-754    |             |Endian Format|             |
+-------------+-------------+-------------+-------------+-------------+
| D           | Double      | 8           | Transmitted |             |
|             |             |             | in Little   |             |
|             | IEEE-754    |             |Endian Format|             |
+-------------+-------------+-------------+-------------+-------------+
| I1          | Signed      | 1           |             |-128 to +127 |
|             | Char        |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| I2          | Signed      | 2           | Transmitted | -32768 to   |
|             | Short       |             | in Little   | 32767       |
|             |             |             |Endian Format|             |
+-------------+-------------+-------------+-------------+-------------+
| I4          | Signed      | 4           | Transmitted | -2^31 to    |
|             | Int         |             | in Little   | 2^31-1      |
|             |             |             |Endian Format|             |
+-------------+-------------+-------------+-------------+-------------+
| I8          | Signed      | 8           | Transmitted | -2^63 to    |
|             | long long   |             | in Little   | 2^63-1      |
|             |             |             |Endian Format|             |
+-------------+-------------+-------------+-------------+-------------+
| SN          | String      | N           | ASCII       |             |
+-------------+-------------+-------------+-------------+-------------+
| B8          | Byte Array  | 8           |B[0]         |             |
|             |             |             |Transmitted  |             |
|             |             |             | first       |             |
+-------------+-------------+-------------+-------------+-------------+

3. Packet Structure
-------------
Below provided description of OpenIMU framework messages. Messages described
the way they occur in serial line. Open IMU framework takes care of wrapping up user 
payload and calculating CRC.       

3.1 Generic Packet Format
~~~~~~~~~~~~~~~~~~~~~~~~~~
All of the Input and Output packets, except the Ping command, conform to
the following structure:

+-------------+-------------+-------------+-------------+-------------+
| 0x5555      || <2-byte    || <payload   || <variable  ||  <2-byte   |
|             || packet code|| byte-length|| length     || CRC (U2)>  |
|             || (U2)>      || (U1)>      || payload>   ||            |
+-------------+-------------+-------------+-------------+-------------+


3.2 Packet Header
~~~~~~~~~~~~~~~~~~~

The packet header is always the bit pattern 0x5555.


3.3 Packet Code
~~~~~~~~~~~~~~~~~~


The packet code is always two bytes long in unsigned short integer
format. Most input and output packet types for convenience can be 
interpreted as a pair of ASCII characters. For example code "aB" will 
translate to hex value 0x6142".    
NOTE: 
    1. First character value should be more or equal 'a' (0x61)
    2. Packet code transmitted in Big Endian format

    
3.4 Payload Length
~~~~~~~~~~~~~~~~~

The payload length is always a one byte unsigned character with a range
of 0-255. The payload length byte is the length (in bytes) of the
*<variable length payload>* portion of the packet ONLY, and does not
include the CRC.

3.5 Payload
~~~~~~~~~~~

The payload is of variable length based on the packet type.

3.6 16-bit CRC-CCITT
~~~~~~~~~~~~~

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

See Appendix A for sample code that implements the 16-bit CRC algorithm.


3.6 NAK Packet
~~~~~~~~~~~~~

NAK packet sent in response to the unknown or corrupted input message.
NAK packet has next format:
 
+-------------+-------------+-------------+-------------+-------------+
| 0x5555      || 0x0000     || 2          || code of    ||  <2-byte   |
|             |             |             || received   || CRC (U2)>  |
|             |             |             || packet or 0|             |
+-------------+-------------+-------------+-------------+-------------+
 

4. Messaging Overview
------------------

Table below summarizes the messages initially introduced in OpenIMU300xx framework.
New messages can be easily added (please check chapter "Procedure for adding new message")  
Packet codes are assigned mostly using the ASCII mnemonics defined above
and are indicated in the summary table below and in the detailed
sections for each command. The payload byte-length is often related to
other data elements in the packet as defined in the table below. The
referenced variables are defined in the detailed sections following.
Output messages are sent from the OpenIMU Series inertial system to the
user system as a result of user request or a continuous packet output
setting. Interactive messages can be sent from the user system to the OpenIMU
Series inertial system and will result in an associated Reply Message or
NAK message. Note that reply messages typically have the same **<2-byte
packet type (U2)>** as the input message that evoked it but with a
different payload.


     **Messages Table**
+---------------+-----------+-------------+--------------+-------------+
|  **ASCII**    || **Code** || **Payload**|  **Function**|   **Type**  |
|               || **(U2)** || **Length** |              |             |
|               |           || **(U1)**   |              |             |
+---------------+-----------+-------------+--------------+-------------+
|| **Interactive**                                                     |
|| **Messages**                                                        |
+---------------+-----------+-------------+--------------+-------------+
| pG            | 0x7047    | 0           ||  Ping       || Input/Reply|
|               |           |             ||             || Message    |
+---------------+-----------+-------------+--------------+-------------+
| uC            | 0x7543    | N           ||   Update    || Input/Reply|
|               |           | (up to 248) ||   Config    || Message    |
|               |           |             ||   Command/  ||            |
|               |           |             ||   Response  ||            |
+---------------+-----------+-------------+--------------+-------------+
| uP            | 0x7550    | 12          ||  Update     || Input/Reply|
|               |           |             ||  Parameter  || Message    |
|               |           |             ||  Command/   ||            |
|               |           |             ||  Response   ||            |
+---------------+-----------+-------------+--------------+-------------+
| uA            | 0x7541    | N           ||  Update     || Input/Reply|
|               |           | (up to 240) ||  All        || Message    |
|               |           |             ||  Command/   ||            |
|               |           |             ||  Response   ||            |
+---------------+-----------+-------------+--------------+-------------+
| sC            | 0x7343    | 0           ||  Save       || Input/Reply|
|               |           |             ||  Config     || Message    |
|               |           |             ||  Command/   ||            |
|               |           |             ||  Response   ||            |
+---------------+-----------+-------------+--------------+-------------+
| rD            | 0x7244    | 0           ||  Restore    || Input/Reply|
|               |           |             ||  Defaults   || Message    |
|               |           |             ||  Command/   ||            |
|               |           |             ||  Response   ||            |
+---------------+-----------+-------------+--------------+-------------+
| sC            | 0x7343    | 0           ||  Save       || Input/Reply|
|               |           |             ||  Config     || Message    |
|               |           |             ||  Command/   ||            |
|               |           |             ||  Response   ||            |
+---------------+-----------+-------------+--------------+-------------+
| gC            | 0x6743    | 8           ||  Get        || Input/Reply|
|               |           |             ||  Config     || Message    |
|               |           |             ||  Command/   ||            |
|               |           |             ||  Response   ||            |
+---------------+-----------+-------------+--------------+-------------+
| gP            | 0x6750    | 4           ||  Get        || Input/Reply|
|               |           |             ||  Parameter  || Message    |
|               |           |             ||  Command/   ||            |
|               |           |             ||  Response   ||            |
+---------------+-----------+-------------+--------------+-------------+
| gA            | 0x6741    | 0           ||  Get        || Input/Reply|
|               |           |             ||  All Params || Message    |
|               |           |             ||  Command/   ||            |
|               |           |             ||  Response   ||            |
+---------------+-----------+-------------+--------------+-------------+
| gV            | 0x6756    | N           ||  Get        || Input/Reply|
|               |           |             ||  Version    || Message    |
|               |           |             ||  Command/   ||            |
|               |           |             ||  Response   ||            |
+---------------+-----------+-------------+--------------+-------------+
|| **Output**                                                          |
|| **Messages**                                                        |
+---------------+-----------+-------------+--------------+-------------+
| zT            | 0x7a54    | 4           ||  Counter    || Output     |
|               |           |             ||             || Message    |
+---------------+-----------+-------------+--------------+-------------+
| z1            | 0x7a31    | 40          ||  Scaled     || Output     |
|               |           |             ||  Sensors    || Message    |
|               |           |             ||  Data       ||            |
+---------------+-----------+-------------+--------------+-------------+


5. OpenIMU Interactive Messages
------------

5.1 User Ping Command
~~~~~~~~~~~~~~~~~~~~

+----------------------+-------------+--------+-------------+
| Ping (‘pG’ = 0x7047) |             |        |             |
+----------------------+-------------+--------+-------------+
| Preamble             | Packet Code | Length | Termination |
+----------------------+-------------+--------+-------------+
| 0x5555               | 0x7047      | 0      | <CRC (U2)>  |
+----------------------+-------------+--------+-------------+

The user Ping command has no payload. Sending the Ping command will cause the
unit to send a Ping response with next format:

+----------------------+-------------+--------+-------------------------+-------------+
| Ping (‘pG’ = 0x7047) |             |        |                         |             |
+----------------------+-------------+--------+-------------------------+-------------+
| Preamble             | Packet Type | Length |  Payload                | Termination |
+----------------------+-------------+--------+-------------------------+-------------+
| 0x5555               | 0x7047      |   N    || Unit Model and Serial  | <CRC (U2)>  |
|                      |             |        || Number  <S> (string)   |             |
+----------------------+-------------+--------+-------------------------+-------------+

The user Ping response will return null-terminated string, containing unit model name 
and unit serial number.


5.2 Update Config Command
~~~~~~~~~~~~~~~~~~~~~~

+----------------------+-------------+--------+----------------+-------------+
| (‘uC’ = 0x7543)      |             |        |                |             |
+----------------------+-------------+--------+----------------+-------------+
| Preamble             | Packet Type | Length | Payload        | Termination |
+----------------------+-------------+--------+----------------+-------------+
| 0x5555               | 0x7543      | 8+8*N  | N Parameters   | <CRC (U2)>  |
+----------------------+-------------+--------+----------------+-------------+

The Update Config command used to update and apply N consecutive user-defined 
configuration parameters at a time in unit. Parameter value is 64 bit (8 bytes) 
and can have arbitrary type. 

Update Config Payload Format

+-----------+-----------------+-----------+-----------+
| Byte      | Name            | Format    | Notes     |
| Offset    |                 |           |           |
+-----------+-----------------+-----------+-----------+
| 0         || Number         | U4        | LSB First |
|           || Of             |           |           |
|           || consecutive    |           |           |
|           || parameters     |           |           |
|           || to update      |           |           |
+-----------+-----------------+-----------+-----------+
| 4         || Offset of      | U4        | LSB First |
|           || first          |           |           |
|           || parameter      |           |           |
|           || in unit config |           |           |
|           || structure      |           |           |
+-----------+-----------------+-----------+-----------+
| 8         | Parameter Value || U8 or I8 | LSB First |
|           |                 || or F8 or |           |
|           |                 || double or|           |
|           |                 || S8 or A8 |           |
+-----------+-----------------+-----------+-----------+
|      .........................................      |
+-----------+-----------------+-----------+-----------+
| 8+N*8     |Parameter Value  || U8 or I8 | LSB First |
|           |                 || or F8 or |           |
|           |                 || double or|           |
|           |                 || S8 or A8 |           |
+-----------+-----------------+-----------+-----------+

Upon reception – each parameter is validated (if desired) and if validation passes
parameter gets written into gUserConfiguration structure and also applied to the 
system on-the-fly(if desired). If value of one parameter is invalid – all parameters 
ignored. 
Updated configuration parameters will be active until next unit power cycle or reset. 

Update Config command will have next response:

+----------------------+-------------+--------+----------------+-------------+
| Preamble             | Packet Type | Length | Payload        | Termination |
+----------------------+-------------+--------+----------------+-------------+
| 0x5555               | 0x7543      | 4      | Error Code (I4)| <CRC (U2)>  |
+----------------------+-------------+--------+----------------+-------------+

Error code can be: (0) – “Success”, (-3) – “Invalid Payload Size”, (-1) – “Invalid parameter number”,
(-2) – “Invalid parameter value”


5.3 Update Parameter Command
~~~~~~~~~~~~~~~~~~~~~~~~

+----------------------+-------------+--------+----------------+-------------+
| (‘uP’ = 0x7550)      |             |        |                |             |
+----------------------+-------------+--------+----------------+-------------+
| Preamble             | Packet Type | Length | Payload        | Termination |
+----------------------+-------------+--------+----------------+-------------+
| 0x5555               | 0x7550      | 12     |                | <CRC (U2)>  |
+----------------------+-------------+--------+----------------+-------------+

The Update Parameter command used to update and apply single user-defined 
configuration parameter in unit. Parameter value is 64 bit (8 bytes) and can have
arbitrary type. 


Update Parameter Payload Format

+-----------+-----------------+-----------+-----------+
| Byte      | Name            | Format    | Notes     |
| Offset    |                 |           |           |
+-----------+-----------------+-----------+-----------+
| 0         || Offset of      | U4        | LSB First |
|           || parameter      |           |           |
|           || in unit config |           |           |
|           || structure      |           |           |
+-----------+-----------------+-----------+-----------+
| 8         | Parameter Value || U8 or I8 | LSB First |
|           |                 || or F8 or |           |
|           |                 || double or|           |
|           |                 || S8 or A8 |           |
+-----------+-----------------+-----------+-----------+

Upon reception parameter value is validated (if desired) and if validation passes
parameter gets written into gUserConfiguration structure and also applied to the 
system on-the-fly(if desired). If value of the parameter is invalid – it ignored. 
Updated configuration parameter will be active until next unit power cycle or reset. 

Update Parameter command will have next response:

+----------------------+-------------+--------+----------------+-------------+
| Preamble             | Packet Type | Length | Payload        | Termination |
+----------------------+-------------+--------+----------------+-------------+
| 0x5555               | 0x7550      | 4      | Error Code (I4)| <CRC (U2)>  |
+----------------------+-------------+--------+----------------+-------------+

Error code can be: (0) – “Success”, (-3) – “Invalid Payload Size”, (-1) – “Invalid parameter number”,
(-2) – “Invalid parameter value”



5.4 Update All Command
~~~~~~~~~~~~~~~~~

+----------------------+-------------+--------+------------------------+-------------+
| (‘uA’ = 0x7541)      |             |        |                        |             |
+----------------------+-------------+--------+------------------------+-------------+
| Preamble             | Packet Type | Length | Payload                | Termination |
+----------------------+-------------+--------+------------------------+-------------+
| 0x5555               | 0x7541      | 8N     | N (up to 30) parameters| <CRC (U2)>  |
+----------------------+-------------+--------+------------------------+-------------+

The Update All command used  to update/apply up to 30 consecutive user-defined configuration 
parameters at a time in unit, starting from first parameter in user configuration 
structure. Each parameter has size 8 bytes (64 bit) and can have arbitrary type. 


Update All Payload Format

+-----------+-----------------+-----------+-----------+
| Byte      | Name            | Format    | Notes     |
| Offset    |                 |           |           |
+-----------+-----------------+-----------+-----------+
| 0         | Parameter Value || U8 or I8 | LSB First |
|           |(first parameter)|| or F8 or |           |
|           |                 || double or|           |
|           |                 || S8 or A8 |           |
+-----------+-----------------+-----------+-----------+
|      .........................................      |
+-----------+-----------------+-----------+-----------+
| N*8       |Parameter Value  || U8 or I8 | LSB First |
|           |                 || or F8 or |           |
|           |                 || double or|           |
|           |                 || S8 or A8 |           |
+-----------+-----------------+-----------+-----------+

Upon reception – each parameter is validated (if desired) and if validation passes
parameter gets written into gUserConfiguration structure, starting from first parameter
(offset 0) and also applied to the system on-the-fly(if desired). If value of one parameter 
is invalid – all parameters ignored. First two parameters are ignored. 
Updated configuration parameters will be active until next unit power cycle or reset. 

Update All command will have next response:

+----------------------+-------------+--------+----------------+-------------+
| Preamble             | Packet Type | Length | Payload        | Termination |
+----------------------+-------------+--------+----------------+-------------+
| 0x5555               | 0x7541      | 4      | Error Code (I4)| <CRC (U2)>  |
+----------------------+-------------+--------+----------------+-------------+

Error code can be: (0) – “Success”, (-3) – “Invalid Payload Size”, (-1) – “Invalid parameter number”,
(-2) – “Invalid parameter value”


5.5 Save Config Command
~~~~~~~~~~~~~~~~~~~~

+-----------------------------+-------------+--------+-------------+
| Save Config (‘sc’ = 0x7343) |             |        |             |
+-----------------------------+-------------+--------+-------------+
| Preamble                    | Packet Code | Length | Termination |
+-----------------------------+-------------+--------+-------------+
| 0x5555                      | 0x7343      | 0      | <CRC (U2)>  |
+-----------------------------+-------------+--------+-------------+

The Save Config command has no payload. Upon reception of "Save Config" command unit will
save current gUnitConfiguration structure into EEPROM and updated parameters will be applied to the
unit all the times upon startup (untill new changes will be made).

Save Config command will have next responsein in case of success:


+-----------------------------+-------------+--------+-------------+
| Preamble                    | Packet Code | Length | Termination |
+-----------------------------+-------------+--------+-------------+
| 0x5555                      | 0x7343      | 0      | <CRC (U2)>  |
+-----------------------------+-------------+--------+-------------+

5.5 Restore Defaults
~~~~~~~~~~~~~~~~~~~~

+----------------------------------+-------------+--------+-------------+
| Restore defaults (‘rd’ = 0x7244) |             |        |             |
+----------------------------------+-------------+--------+-------------+
| Preamble                         | Packet Code | Length | Termination |
+----------------------------------+-------------+--------+-------------+
| 0x5555                           | 0x7244      | 0      | <CRC (U2)>  |
+----------------------------------+-------------+--------+-------------+

The Restore defaults command has no payload. Upon reception of "Restore Defaults" command unit will
save default configuration structure gDefaultUserConfig into EEPROM and updated parameters will be applied to the
unit all the times upon startup (untill new changes will be made).

Restore Defaults command will have next responsein in case of success:


+-----------------------------+-------------+--------+-------------+
| Preamble                    | Packet Code | Length | Termination |
+-----------------------------+-------------+--------+-------------+
| 0x5555                      | 0x7244      | 0      | <CRC (U2)>  |
+-----------------------------+-------------+--------+-------------+


5.6 Get Config Command
~~~~~~~~~~~~~~~~~~~~~~

+----------------------+-------------+--------+----------------+-------------+
| (‘gC’ = 0x6743)      |             |        |                |             |
+----------------------+-------------+--------+----------------+-------------+
| Preamble             | Packet Type | Length | Payload        | Termination |
+----------------------+-------------+--------+----------------+-------------+
| 0x5555               | 0x6743      | 8      |                | <CRC (U2)>  |
+----------------------+-------------+--------+----------------+-------------+

The Get Config command used to retrieve N consecutive user-defined 
configuration parameters at a time from unit. Parameter value is 64 bit (8 bytes) 
and can have arbitrary type. 

Get Config Payload Format

+-----------+-----------------+-----------+-----------+
| Byte      | Name            | Format    | Notes     |
| Offset    |                 |           |           |
+-----------+-----------------+-----------+-----------+
| 0         || Number         | U4        | LSB First |
|           || Of             |           |           |
|           || consecutive    |           |           |
|           || parameters     |           |           |
|           || to update      |           |           |
+-----------+-----------------+-----------+-----------+
| 4         || Offset of      | U4        | LSB First |
|           || first          |           |           |
|           || parameter      |           |           |
|           || in unit config |           |           |
|           || structure      |           |           |
+-----------+-----------------+-----------+-----------+


Get Config command will have next response:

+----------------------+-------------+--------+----------------+-------------+
| (‘gC’ = 0x6743)      |             |        |                |             |
+----------------------+-------------+--------+----------------+-------------+
| Preamble             | Packet Type | Length | Payload        | Termination |
+----------------------+-------------+--------+----------------+-------------+
| 0x5555               | 0x6743      | 8+8*N  | N parameters   | <CRC (U2)>  |
+----------------------+-------------+--------+----------------+-------------+


Get Config Response Payload Format in case of success:

+-----------+-----------------+-----------+-----------+
| Byte      | Name            | Format    | Notes     |
| Offset    |                 |           |           |
+-----------+-----------------+-----------+-----------+
| 0         || Number         | U4        | LSB First |
|           || Of             |           |           |
|           || consecutive    |           |           |
|           || parameters     |           |           |
|           || to update      |           |           |
+-----------+-----------------+-----------+-----------+
| 4         || Offset of      | U4        | LSB First |
|           || first          |           |           |
|           || parameter      |           |           |
|           || in unit config |           |           |
|           || structure      |           |           |
+-----------+-----------------+-----------+-----------+
| 8         | Parameter Value || U8 or I8 | LSB First |
|           |                 || or F8 or |           |
|           |                 || double or|           |
|           |                 || S8 or A8 |           |
+-----------+-----------------+-----------+-----------+
|      .........................................      |
+-----------+-----------------+-----------+-----------+
| 8+N*8     |Parameter Value  || U8 or I8 | LSB First |
|           |                 || or F8 or |           |
|           |                 || double or|           |
|           |                 || S8 or A8 |           |
+-----------+-----------------+-----------+-----------+



Get Config Response Payload Format in case of error:

+----------------------+-------------+--------+----------------+-------------+
| Preamble             | Packet Type | Length | Payload        | Termination |
+----------------------+-------------+--------+----------------+-------------+
| 0x5555               | 0x6743      | 4      | Error Code (I4)| <CRC (U2)>  |
+----------------------+-------------+--------+----------------+-------------+

Error code can be: (0) – “Success”, (-3) – “Invalid Payload Size”, (-1) – “Invalid parameter number”,
(-2) – “Invalid parameter value”


5.7 Get All Command
~~~~~~~~~~~~~~~~~~~~~~

+----------------------+-------------+--------+------------+
| (‘gA’ = 0x6741)      |             |        |            |
+----------------------+-------------+--------+------------+
| Preamble             | Packet Type | Length |Termination |
+----------------------+-------------+--------+------------+
| 0x5555               | 0x6741      | 0      |<CRC (U2)>  |
+----------------------+-------------+--------+------------+

The Get All command used to retrieve N (up to 30) consecutive user-defined 
configuration parameters at a time from unit, starting from first parameter in gUserConfiguration
structure. Parameter value is 64 bit (8 bytes) and can have arbitrary type. 



Get All command will have next response:

+----------------------+-------------+--------+----------------+-------------+
| Preamble             | Packet Type | Length | Payload        | Termination |
+----------------------+-------------+--------+----------------+-------------+
| 0x5555               | 0x6741      | 8*N    |  N parameters  | <CRC (U2)>  |
+----------------------+-------------+--------+----------------+-------------+


Get Config Response Payload Format in case of success:

+-----------+-----------------+-----------+-----------+
| Byte      | Name            | Format    | Notes     |
| Offset    |                 |           |           |
+-----------+-----------------+-----------+-----------+
| 0         || Number         | U4        | LSB First |
|           || Of             |           |           |
|           || consecutive    |           |           |
|           || parameters     |           |           |
|           || to update      |           |           |
+-----------+-----------------+-----------+-----------+
| 4         || Offset of      | U4        | LSB First |
|           || first          |           |           |
|           || parameter      |           |           |
|           || in unit config |           |           |
|           || structure      |           |           |
+-----------+-----------------+-----------+-----------+
| 8         | Parameter Value || U8 or I8 | LSB First |
|           |                 || or F8 or |           |
|           |                 || double or|           |
|           |                 || S8 or A8 |           |
+-----------+-----------------+-----------+-----------+
|      .........................................      |
+-----------+-----------------+-----------+-----------+
| 8+N*8     |Parameter Value  || U8 or I8 | LSB First |
|           |                 || or F8 or |           |
|           |                 || double or|           |
|           |                 || S8 or A8 |           |
+-----------+-----------------+-----------+-----------+



Get All Response Payload Format in case of error:

+-----------------+-------------+--------+----------------+-------------+
| Preamble        | Packet Type | Length | Payload        | Termination |
+-----------------+-------------+--------+----------------+-------------+
| 0x5555          | 0x6741      | 4      | Error Code (I4)| <CRC (U2)>  |
+-----------------+-------------+--------+----------------+-------------+

Error code can be: (0) – “Success”, (-3) – “Invalid Payload Size”, (-1) – “Invalid parameter number”,
(-2) – “Invalid parameter value”


5.8 Get Parameter Command
~~~~~~~~~~~~~~~~~~~~~~

+----------------------+-------------+--------+----------------+-------------+
| (‘gP’ = 0x6750)      |             |        |                |             |
+----------------------+-------------+--------+----------------+-------------+
| Preamble             | Packet Type | Length | Payload        | Termination |
+----------------------+-------------+--------+----------------+-------------+
| 0x5555               | 0x6750      | 4      |                | <CRC (U2)>  |
+----------------------+-------------+--------+----------------+-------------+

The Get Parameter command used to retrieve one user-defined configuration parameter
from unit gUserConfiguration structure. Parameter value is 64 bit (8 bytes) and 
can have arbitrary type. 

Get Parameter command payload format

+-----------+-----------------+-----------+-----------+
| Byte      | Name            | Format    | Notes     |
| Offset    |                 |           |           |
+-----------+-----------------+-----------+-----------+
| 0         || Offset of      | U4        | LSB First |
|           || parameter      |           |           |
|           || in unit config |           |           |
|           || structure      |           |           |
+-----------+-----------------+-----------+-----------+


Get Parameter command will have next response:

+----------------------+-------------+--------+----------------+-------------+
| (‘gP’ = 0x6750)      |             |        |                |             |
+----------------------+-------------+--------+----------------+-------------+
| Preamble             | Packet Type | Length | Payload        | Termination |
+----------------------+-------------+--------+----------------+-------------+
| 0x5555               | 0x6750      | 12     |  parameter     | <CRC (U2)>  |
+----------------------+-------------+--------+----------------+-------------+


Get Parameter response rayload format in case of success:

+-----------+-----------------+-----------+-----------+
| Byte      | Name            | Format    | Notes     |
| Offset    |                 |           |           |
+-----------+-----------------+-----------+-----------+
| 0         || Offset of      | U4        | LSB First |
|           || parameter      |           |           |
|           || in unit config |           |           |
|           || structure      |           |           |
+-----------+-----------------+-----------+-----------+
| 4         | Parameter Value || U8 or I8 | LSB First |
|           |                 || or F8 or |           |
|           |                 || double or|           |
|           |                 || S8 or A8 |           |
+-----------+-----------------+-----------+-----------+



Get Parameter response payload format in case of error:

+----------------------+-------------+--------+----------------+-------------+
| Preamble             | Packet Type | Length | Payload        | Termination |
+----------------------+-------------+--------+----------------+-------------+
| 0x5555               | 0x6750      | 4      | Error Code (I4)| <CRC (U2)>  |
+----------------------+-------------+--------+----------------+-------------+

Error code can be: (0) – “Success”, (-3) – “Invalid Payload Size”, (-1) – “Invalid parameter number”,
(-2) – “Invalid parameter value”


5.9 Get User Version Command
~~~~~~~~~~~~~~~~~~~~

+----------------------+-------------+--------+-------------+
| (‘gV’ = 0x6756)      |             |        |             |
+----------------------+-------------+--------+-------------+
| Preamble             | Packet Code | Length | Termination |
+----------------------+-------------+--------+-------------+
| 0x5555               | 0x6756      | 0      | <CRC (U2)>  |
+----------------------+-------------+--------+-------------+

The Get Version command has no payload. Sending the Get Version command will cause the
unit to send a response with next format:

+----------------------+-------------+--------+-------------------------+-------------+
| Preamble             | Packet Type | Length |  Payload                | Termination |
+----------------------+-------------+--------+-------------------------+-------------+
| 0x5555               | 0x6756      |   N    || User Version String    | <CRC (U2)>  |
+----------------------+-------------+--------+-------------------------+-------------+

The Get Version response will return null-terminated string, user version. User version string
defined in the UserMessaging.c file.


6. OpenIMU Output messages
------------

Below provided examples of OpenIMU output messages implemented in OpenImu framework.
Users can easily add new messages or discard these examples at their discretion.
Output messages are to be continuously sent out by unit with preconfigured message rate.

 
6.1 User Test Message
~~~~~~~~~~~~~~~~~~~~~~

User Test output message has next format:

+----------------------+-------------+--------+----------------+-------------+
|   (‘zT’ = 0x7a54)    |             |        |                |             |
+----------------------+-------------+--------+----------------+-------------+
| Preamble             | Packet Type | Length | Payload        | Termination |
+----------------------+-------------+--------+----------------+-------------+
| 0x5555               | 0x7a54      |   4    |                | <CRC (U2)>  |
+----------------------+-------------+--------+----------------+-------------+


User Test Message payload has next format:

+-----------+-----------------+-----------+-----------+
| Byte      | Name            | Format    | Notes     |
| Offset    |                 |           |           |
+-----------+-----------------+-----------+-----------+
| 0         |  Counter        | U4        | LSB First |
+-----------+-----------------+-----------+-----------+
 
Counter is simple message counter which will increase by 1 with in every consecutive Test message 


6.2 User Sensors Data Message
~~~~~~~~~~~~~~~~~~~~~~

User Sensors Data  message has next format:

+----------------------+-------------+--------+----------------+-------------+
|   (‘z1’ = 0x7a31)    |             |        |                |             |
+----------------------+-------------+--------+----------------+-------------+
| Preamble             | Packet Type | Length | Payload        | Termination |
+----------------------+-------------+--------+----------------+-------------+
| 0x5555               | 0x7a31      |   40   |                | <CRC (U2)>  |
+----------------------+-------------+--------+----------------+-------------+


User Sensors Data Message payload has next format:

+-----------+-------------------+-----------+-----------+
| Byte      | Name              | Format    | Notes     |
| Offset    |                   |           |           |
+-----------+-------------------+-----------+-----------+
| 0         || System Timer at  | U4        | LSB First |
|           || the moment of    |           |           |
|           || sensors sampling |           |           |
+-----------+-------------------+-----------+-----------+
|  4        || Acceleration     | F4        | LSB First |
|           || value for axis X |           |           |
|           || (in G)           |           |           |
+-----------+-------------------+-----------+-----------+
|  8        || Acceleration     | F4        | LSB First |
|           || value for axis Y |           |           |
|           || (in G)           |           |           |
+-----------+-------------------+-----------+-----------+
|  12       || Acceleration     | F4        | LSB First |
|           || value for axis Z |           |           |
|           || (in G)           |           |           |
+-----------+-------------------+-----------+-----------+
|  16       || Rotation speed   | F4        | LSB First |
|           || for axis X (dps) |           |           |
+-----------+-------------------+-----------+-----------+
|  20       || Rotation speed   | F4        | LSB First |
|           || for axis Y (dps) |           |           |
+-----------+-------------------+-----------+-----------+
|  24       || Rotation speed   | F4        | LSB First |
|           || for axis Z (dps) |           |           |
+-----------+-------------------+-----------+-----------+
|  28       || Magnetic field   | F4        | LSB First |
|           || for axis X (G)   |           |           |
+-----------+-------------------+-----------+-----------+
|  32       || Magnetic field   | F4        | LSB First |
|           || for axis Y (G)   |           |           |
+-----------+-------------------+-----------+-----------+
|  36       || Magnetic field   | F4        | LSB First |
|           || for axis Z (G)   |           |           |
+-----------+-------------------+-----------+-----------+


6.3 User Arbitrary Data Message
~~~~~~~~~~~~~~~~~~~~~~

User Arbitrary Data  message has next format:

+----------------------+-------------+--------+----------------+-------------+
|   (‘z2’ = 0x7a32)    |             |        |                |             |
+----------------------+-------------+--------+----------------+-------------+
| Preamble             | Packet Type | Length | Payload        | Termination |
+----------------------+-------------+--------+----------------+-------------+
| 0x5555               | 0x7a32      |   27   |                | <CRC (U2)>  |
+----------------------+-------------+--------+----------------+-------------+


User Arbitrary Data Message payload has next format:

+-----------+---------------------+-----------+-----------+
| Byte      | Name                | Format    | Notes     |
| Offset    |                     |           |           |
+-----------+---------------------+-----------+-----------+
|  0        || System Timer at    | U4        | LSB First |
|           || the moment of      |           |           |
|           || sensors sampling   |           |           |
+-----------+---------------------+-----------+-----------+
|  4        | Data of type Byte   | U1        |           |
+-----------+---------------------+-----------+-----------+
|  5        | Data of type short  | I2        | LSB First |
+-----------+---------------------+-----------+-----------+
|  7        | Data of type int    | I4        | LSB First |
+-----------+---------------------+-----------+-----------+
|  11       | Data of type int64  | I8        | LSB First |
+-----------+---------------------+-----------+-----------+
|  19       | Data of type double | D4        | LSB First |
+-----------+---------------------+-----------+-----------+


7 Steps to create your own interactive or output packet in embedded OpenIMU software framework
-------------------
 
User packet processing engine located in the file UserMessaging.c.

7.1 To create new interactive packet: 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Add new input packet type into the enumerator structure **UserInPacketType** in the file **UserMessaging.h** before USR_IN_MAX

::

       typedef enum {
	USR_IN_NONE         = 0 ,
	USR_IN_PING             ,     
	USR_IN_UPDATE_CONFIG    ,
	USR_IN_UPDATE_PARAM     ,
	USR_IN_UPDATE_ALL       ,
	USR_IN_SAVE_CONFIG      ,
	USR_IN_RESTORE_DEFAULTS ,
	USR_IN_GET_CONFIG       ,
	USR_IN_GET_PARAM        ,
	USR_IN_GET_ALL          ,
	USR_IN_GET_VERSION      ,
	// add new packet type here, before USR_IN_MAX
	USR_IN_MAX              ,
       }UserInPacketType;


2. Add new packet type and code into the structure **UserInputPackets** in the file **UserMessaging.c**. Packet code consists of 
   two bytes and can be chosen arbitrary, but first byte SHOULD have value more or equal **0x61**.

::

     usr_packet_t userInputPackets[] = {		//       
	{USR_IN_NONE,               {0,0}},   //  "  "
	{USR_IN_PING,               "pG"}, 
	{USR_IN_UPDATE_CONFIG,      "uC"}, 
	{USR_IN_UPDATE_PARAM,       "uP"}, 
	{USR_IN_UPDATE_ALL,         "uA"}, 
	{USR_IN_SAVE_CONFIG,        "sC"}, 
	{USR_IN_RESTORE_DEFAULTS,   "rD"}, 
	{USR_IN_GET_CONFIG,         "gC"}, 
	{USR_IN_GET_PARAM,          "gP"}, 
	{USR_IN_GET_ALL,            "gA"}, 
	{USR_IN_GET_VERSION,        "gV"}, 
     // place new input packet code here, before USR_IN_MAX
	{USR_IN_MAX,                {0xff, 0xff}},   //  "" 
     };
     


3. Add code which handles input packet into the function **HandleUserInputPacket** in the file **UserMessaging.c** . As a part of packet handling 
   fill up desired response payload (starting from address ptrUcbPacket->payload) and provide response payload length in the parameter 
   ptrUcbPacket->payloadLength. If no response payload required – provide payload length of 0. The packet code in the response will be 
   the same as in the command. If erroneous conditions discovered during packet processing – set valid variable to FALSE so sustem will 
   respond with NAK packet. Additional diagnostics in arbitrary format can be provided in the response payload (see uP packet example above).  
::

	case USR_IN_UPDATE_PARAM:
             UpdateUserParam((userParamPayload*)ptrUcbPacket->payload, &ptrUcbPacket->payloadLength);
             break;



4. Done

7.2 To create new output packet: 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 
1. Add new output packet type into the enumerator structure UserOutPacketType in the file **UserMessaging.h**

::

    // User input packet codes, change at will
    typedef enum {
	USR_OUT_NONE  = 0,  // 0
	USR_OUT_TEST,       // 1
	USR_OUT_DATA1 ,     // 2            
	USR_OUT_DATA2 ,     // 2            
    // add new output packet type here, before USR_OUT_MAX    
	USR_OUT_MAX
    }UserOutPacketType;
    

2. Add new packet type and code into the structure **UserOutputPackets** in the file **UserMessaging.c**. Packet code can be chosen arbitrary, 
but first byte SHOULD have value more or equal 0x61 and the packet code should be unique among input and output packets.

::

    // packet codes here should be unique - 
    // should not overlap codes for input packets and system packets
    // First byte of Packet code should have value  >= 0x61  
    usr_packet_t userOutputPackets[] = {	
    //   Packet Type                Packet Code
	{USR_OUT_NONE,              {0x00, 0x00}}, 
	{USR_OUT_TEST,              "zT"},   
	{USR_OUT_DATA1,             "z1"},   
	{USR_OUT_DATA2,             "z2"},   
    // place new type and code here
	{USR_OUT_MAX,               {0xff, 0xff}},   //  "" 
    };
    

3. Add code which handles input packet into the function HandleUserOutputPacket in the file UserMessaging.c. Fill up desired packet payload
(starting from address payload) and provide response payload length in the parameter payloadLen. If no response payload required – provide payload length of 0.

::

 case USR_OUT_DATA1:
     {   int n = 0;
         double accels[3];
         double mags[3];
         double rates[3];
         data1_payload_t *pld = (data1_payload_t *)payload;  

         pld->timer  = getDacqTime();
         GetAccelsData(accels);
         for (int i = 0; i < 3; i++, n++){
             pld->sensorsData[n] = (float)accels[i];
         }
         GetRatesData(rates);
         for (int i = 0; i < 3; i++, n++){
             pld->sensorsData[n] = (float)rates[i];
         }
         GetMagsData(mags);
         for (int i = 0; i < 3; i++, n++){
             pld->sensorsData[n] = (float)mags[i];
         }
         *payloadLen = sizeof(data1_payload_t);
     }


4. To activate output of the packet use function SetUserPacketType in file UserMessaging.c  and provide desired packet type as a parameter. Or provide output packet
type and packet rate in default user configuration in filr UserConfiguration.c. Output of specific packet can also be changed "on-the-fly" by sending to unit
command "uP" with parameter number 3 and desired parameter value. Output packet rate can be changed "on-the-fly " by sending to unit command "uP" with parameter
number 4 and desired parameter value.
   
::
   
    // Default user configuration structure
    // Saved into EEPROM of first startup after reloading the code
    // or as a result of processing "rD" command
    // Do Not remove - just add extra parameters if needed
    // Change default settings  if desired
    const UserConfigurationStruct gDefaultUserConfig = {
	.dataCRC             =  0,
	.dataSize            =  sizeof(UserConfigurationStruct),
	.userUartBaudRate    =  57600,  
	.userPacketType      =  "zT",  
	.userPacketRate      =  0,  
	.lpfAccelFilterFreq  =  50,
	.lpfRateFilterFreq   =  50,
	.orientation         =  "+X+Y+Z"
	// add default parameter values here, if desired
    } ;
       

5. Done

 
