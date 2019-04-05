CAN DBC Get Request Messages
*******************************

.. contents:: Contents
    :local:

.. toctree::
    :maxdepth: 1

Get Requests
------------

Get requests are used by other ECUs in the network to retrieve information from the OpenIMU300RI.
All Get requests are formed as a Request message as specified earlier.  They are used to request
PGNs which have a transmission rate of "On Request".  The format and content of the Request
message is specified in J1939-21 Section 5.4.2.  The user can modify the existing requests and
responses and add requests and responses as needed.  The following Get Requests are implemented
in the example application.


.. table::  *Get Requests*
    :align: left

    +-------------------------------+-------------+-------------+--------------+
    | **Request**                   || **PF**     || **PS**     || **Payload** |
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
        "Set Bank of PS Numbers for Bank0/Bank1" commands.  The given values are the pre-defined values.
    *   Responses have the same PF+PS values as the request


Responses to Get Requests
--------------------------
The following table describe the payloads for responses to Get Requests

.. table::    *ECU Response Message ID Payload*
    :align: left

    +----------+---------------------------------+-------------+
    | **Byte** | **Contents**                    | **Acronym** |
    +----------+---------------------------------+-------------+
    | 0        || bits 0:2 - Priority            || Prio       |
    |          || bit  3   - Extended Data Page  || EDP        |
    |          || bit  4   - Data Page           || DP         |
    |          || bits 5:7 - Reserved Bits       || R          |
    +----------+---------------------------------+-------------+
    | 1        | PDU Format                      | PF          |
    +----------+---------------------------------+-------------+
    | 2        | PDU Specific                    | PS          |
    +----------+---------------------------------+-------------+
    | 3        | Source Address                  | SA          |
    +----------+---------------------------------+-------------+



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
