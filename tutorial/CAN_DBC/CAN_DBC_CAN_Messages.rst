CAN DBC Example Application CAN Messages
******************************************

.. contents:: Contents
    :local:
    :hidden:


Users can keep the implemented messages as is, modify them, or add new messages.

The CAN DBC Example Application does not currently support reception of DBC messages.  It only sends data
packets at a rate that is controlled internally.

.. note::

    Every multi-byte field is sent LSB first, followed by the next higher order bytes.


DBC File Transmit buffer
------------------------

The following table describes the Transmit Buffer used in the DBC application

*   Source files:

    *   The descriptions here are derived from the typedefs and '#define' literal constants in the header files *dbc_file.h*
        in the '*lib/DBC/include*' directory and *stm32f4xx_can.h* in the '*STI32404 MCU Library/include*' directory.
*   Field Definitions and Usage:

    *   The *Standard Identifier* (StdId) is used to identify the message.  See the second table for the identifiers
    *   The "Extended Identifier* (ExtId) is 0 in all messages
    *   "U32" is an unsigned 32-bit integer that can contain value in this range [0..4294967296]
    *   "U8" is an unsigned 8-bit integer that can contain value in this range [0..255]

+--------------------+-------------+------------------+---------------------+-------------------+
| **Byte Number(s)** | **Type**    | **Identifier**   | **Description**     | **Range**         |
+--------------------+-------------+------------------+---------------------+-------------------+
|  0:3               | U32         | StdId            | Standard Identifier | [0..2047]         |
+--------------------+-------------+------------------+---------------------+-------------------+
|  4:7               | U32         | ExtId            | Extended Identifier | [0..536870911]    |
+--------------------+-------------+------------------+---------------------+-------------------+
|  8                 | U8          | IDE              | Message Type        || 0 for Standard   |
|                    |             |                  |                     || 4 for Extended   |
+--------------------+-------------+------------------+---------------------+-------------------+
|  9                 | U8          | RTR              | Frame Type          || 0 for Data Frame |
|                    |             |                  |                     || 2 for Remote     |
+--------------------+-------------+------------------+---------------------+-------------------+
| 10                 | U8          | DLC              | Frame Length        | [0..8]            |
+--------------------+-------------+------------------+---------------------+-------------------+
| 11:18              | U8          | Data             | Frame Data          | each [0..255]     |
+--------------------+-------------+------------------+---------------------+-------------------+

DBC Application Message types
-----------------------------

The following table provides the DBC application message types with
their associated *Standard Identifier* values used to identify the message and the payload length in bytes of the message.

+---------------------+-------------------------------+-----------------------------+----------------+
| *Message Type**     | *Standard Identifier Literal* | *Standard Identifier value* | Payload Length |
+---------------------+-------------------------------+-----------------------------+----------------+
| *Rate Data*         | DBC_MESSAGE_RATE_ID           | 91                          | 8              |
+---------------------+-------------------------------+-----------------------------+----------------+
| *Acceleration Data* | DBC_MESSAGE_ACCEL_ID          | 92                          | 8              |
+---------------------+-------------------------------+-----------------------------+----------------+
| *Euler Angle Data*  | DBC_MESSAGE_ANGLE_ID          | 90                          | 8              |
+---------------------+-------------------------------+-----------------------------+----------------+


    The following data structure is used for all *Signal Definition* messages.  Note that the X/Y/Z values are 16 bit despite the
    fact that they are stored as a 64 bit unsigned integer

    .. code-block:: c


        typedef union
        {
            struct
            {
                uint64_t  x_value  : 16; // 16-bit X-axis value
                uint64_t  y_value  : 16; // 16-bit Y-axis value
                uint64_t  z_value  : 16; // 16-bit Z-axis value
                uint64_t  counter  : 4;  // 4-bit counter incremented with each Tx, rolling over at 16
                uint64_t  reserved : 4;  // ignored
                uint64_t  csum     : 8;  // 8-bit checksum
            } b;
            uint64_t r;
        } DBC_SG_PAYLOAD;

*Signal Definition* (SG_) message Layout
--------------------------------------------------------------------

    +--------------------+------------------------------+
    | **Byte Number(s)** | **Description**              |
    +--------------------+------------------------------+
    | 0:1                | 2-byte X value (LSB first)   |
    +--------------------+------------------------------+
    | 2:3                | 2-byte Y value (LSB first)   |
    +--------------------+------------------------------+
    | 4:5                | 2-byte Z value (LSB first)   |
    +--------------------+------------------------------+
    | 6                  || bits 0:3 counter            |
    |                    || bits 4:7 reserved - ignored |
    +--------------------+------------------------------+
    | 7                  | 8-bit checksum of the first  |
    |                    | seven bytes of the payload   |
    +--------------------+------------------------------+


*Rate Data* message - treated as a "Signal Definition  (SG_) message
--------------------------------------------------------------------

    *Rate Data Payload*

    +--------------------+---------------------------------+
    | **Byte Number(s)** | **Description**                 |
    +--------------------+---------------------------------+
    | 0:1                | 2-byte X rate value (LSB first) |
    +--------------------+---------------------------------+
    | 2:3                | 2-byte Y rate value (LSB first) |
    +--------------------+---------------------------------+
    | 4:5                | 2-byte Z rate value (LSB first) |
    +--------------------+---------------------------------+
    | 6                  | As In Signal Definition Layout  |
    +--------------------+---------------------------------+
    | 7                  | As In Signal Definition Layout  |
    +--------------------+---------------------------------+


*Acceleration Data* message - treated as a "Signal Definition* (SG_) message
---------------------------------------------------------------------------------

    +--------------------+-----------------------------------------+
    | **Byte Number(s)** | **Description**                         |
    +--------------------+-----------------------------------------+
    | 0:1                | 2-byte X acceleration value (LSB first) |
    +--------------------+-----------------------------------------+
    | 2:3                | 2-byte Y acceleration value (LSB first) |
    +--------------------+-----------------------------------------+
    | 4:5                | 2-byte Z acceleration value (LSB first) |
    +--------------------+-----------------------------------------+
    | 6                  | As In Signal Definition Layout          |
    +--------------------+-----------------------------------------+
    | 7                  | As In Signal Definition Layout          |
    +--------------------+-----------------------------------------+

*Euler Angle Data* message data structure
-----------------------------------------

    The following  structure is used for the *Euler Angle Data* message

    .. code-block:: c

        typedef union
        {
            struct {
                uint64_t  pitch     : 24;     // 16-bit X-axis value
                uint64_t  roll      : 24;     // 16-bit Y-axis value
                uint64_t  reserved  : 16;     // ignored
            } b;
            uint64_t r;
        } DBC_ANGLE_PAYLOAD;

*Euler Angle Data* message layout
-----------------------------------------

    +-----------------+--------------------+
    | **Byte Number** | **Description**    |
    +-----------------+--------------------+
    | 0               | LSB of pitch       |
    +-----------------+--------------------+
    | 1               | 2nd LSB of pitch   |
    +-----------------+--------------------+
    | 2               | MSB of pitch (0)   |
    +-----------------+--------------------+
    | 3               | LSB of roll        |
    +-----------------+--------------------+
    | 4               | 2nd LSB of roll    |
    +-----------------+--------------------+
    | 5               | MSB of roll (0)    |
    +-----------------+--------------------+
    | 6:7             | reserved - ignored |
    +-----------------+--------------------+
