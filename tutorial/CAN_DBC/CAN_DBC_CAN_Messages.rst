CAN DBC Example Application CAN Messages
******************************************

Users can keep the implemented messages as is, modify them, or add new messages.

The CAN DBC Example Application does not currently support reception of DBC messages.  It only sends data
packets at a rate that is controlled internally.

.. note::

    Every multi-byte field is sent LSB first, followed by the next higher order bytes.


**Transmit Packet Description**


.. table::


+--------------------+-------------+------------------+---------------------+-------------------+
| **Byte Number(s)** || **Type**   | **Identifier**   | **Description**     | **Range**         |
|                    || (see note) |                  |                     |                   |
+--------------------+-------------+------------------+---------------------+-------------------+
|  0:3               | U32         | StdId            | Standard Identifier | [0..2047]         |
+--------------------+-------------+------------------+---------------------+-------------------+
|  4:7               | U32         | ExtId (see note) | Extended Identifier | [0..536870911]    |
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

**Notes**

    **Type**
        "U32" is an unsigned 4 byte integer with range [0..4294967296]
        "U8" is an unsigned 1 byte integer with range [0..255]
    **ExtId**
        *ExtId* is 0 in all messages

The *Standard Identifier* is used to identify the message.  The following *Standard Identifier* values are used

+---------------------+-------------------------------+-----------------------------+
| *Message Type**     | *Standard Identifier Literal* | *Standard Identifier value* |
+---------------------+-------------------------------+-----------------------------+
| "Rate Data*         | DBC_MESSAGE_RATE_ID           | 91                          |
+---------------------+-------------------------------+-----------------------------+
| "Acceleration Data* | DBC_MESSAGE_ACCEL_ID          | 92                          |
+---------------------+-------------------------------+-----------------------------+
| *Euler Angle Data*  | DBC_MESSAGE_ANGLE_ID          | 90                          |
+---------------------+-------------------------------+-----------------------------+
