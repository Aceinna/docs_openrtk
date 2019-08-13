CAN J1939 Get Request Messages
*******************************

.. contents:: Contents
    :local:

.. toctree::
    :maxdepth: 1

Get Requests
------------

Get requests are used by other ECUs in the network
to retrieve information from the OpenIMU300RI.  All Get requests are formed as a
Request message as specified earlier.
The format and content of the Request message has next format:

Extended header:

    | PF      : 234,
    | PS      : Destination Address,
    | DLC     : 3,
    | Priority: 6,
    | PGN     : 59904.

.. table::    *Request Payload*
    :align: left

    +-----------+---------------------------+
    | **Byte**  | **Description**           |
    +-----------+---------------------------+
    | 0         | N/A                       |
    +-----------+---------------------------+
    | 1         | PF of requested parameter |
    +-----------+---------------------------+
    | 2         | PS of requested parameter |
    +-----------+---------------------------+

  
In table below provided list of the parameters which can be requested from ECU, including their PF, PS and payload length of response messages


.. table::  *List of ECU parameters available for Requests*
    :align: left

    +-------------------------------+-------------+-------------+--------------+
    | **Parameter**                 || **PF**     || **PS**     || **Payload** |
    |                               || (dec)      || (dec)      | **Length**   |
    |                               ||            || (See note) | (bytes)      |
    +-------------------------------+-------------+-------------+--------------+
    | *Software Version*            | 254         | 218         | 5            |
    +-------------------------------+-------------+-------------+--------------+
    | *ECU ID*                      | 253         | 197         | 8            |
    +-------------------------------+-------------+-------------+--------------+
    | *Packet Rate*                 | 255         | 85          | 2            |
    +-------------------------------+-------------+-------------+--------------+
    | *Packet Type*                 | 255         | 86          | 2            |
    +-------------------------------+-------------+-------------+--------------+
    | *Digital Cutoff Frequency*    | 255         | 87          | 3            |
    +-------------------------------+-------------+-------------+--------------+
    | *Orientation*                 | 255         | 88          | 3            |
    +-------------------------------+-------------+-------------+--------------+

.. note::

    *   Provided PS values for all but the *Get Software Version* and *Get ECU ID* can be changed by the
        "Set Bank of PS Numbers for Bank1" command.  The given values are the default values.
    *   In responses values of PF and PS field in extended headers have the same PF+PS values as requested.

Responses to Get Requests
--------------------------
The following table describe the payloads for responses to Get Requests


.. table::    *Software Version Response Payload*
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

.. table::    *ECU ID 64 Bit Response Payload*
    :align: left

    +--------------+-------------------------+
    |   **Bits**   |   **Contents**          |
    +--------------+-------------------------+
    ||  bits 0     || Arbitrary Address      |
    ||  bit  1:3   || Industry Group         |
    ||  bit  4:7   || Vehicle System Instance|
    ||  bits 8:14  || System Bits            |
    ||  bits 15    || Reserved               |
    ||  bits 16:23 || Function Bits          |
    ||  bits 24:28 || Function Instance      |
    ||  bits 29:31 || ECU Bits               |
    ||  bits 32:42 || Manufacturer code      |
    ||  bits 43:63 || ID bits                |
    +--------------+-------------------------+

.. table::  *Packet Rate Response Payload*

    +-----------+-----------------------+
    | **Byte**  | **Description**       |
    +-----------+-----------------------+
    | 0         | Source Address        |
    +-----------+-----------------------+
    | 1         | Output Data Rate      |
    +-----------+-----------------------+

.. table::  *Packet Type Response Payload*

    +-----------+-----------------------+
    | **Byte**  | **Description**       |
    +-----------+-----------------------+
    | 0         | Source Address        |
    +-----------+-----------------------+
    | 1         | Packet Types Bitmask  |
    +-----------+-----------------------+


.. table:: *Digital Cutoff Frequency Response Payload*

    +-----------+-----------------------+
    | **Byte**  | **Description**       |
    +-----------+-----------------------+
    | 0         | Source Address        |
    +-----------+-----------------------+
    | 1         | Acceleration Cutoff   |
    +-----------+-----------------------+
    | 2         | Angular Rate Cutoff   |
    +-----------+-----------------------+

.. table:: *Orientation Response Payload*

    +-----------+----------------------------------+
    | **Byte**  | **Description**                  |
    +-----------+----------------------------------+
    | 0         | Source Address                   |
    +-----------+----------------------------------+
    | 1         | First Byte of Orientation Value  |
    +-----------+----------------------------------+
    | 2         | Second Byte of Orientation Value |
    +-----------+----------------------------------+

.. note::

    *   For Orientation, Cutoff Frequencies Packet Type and Packet Rate responses values of parameters will be the same as in the set commands for these parameters.
