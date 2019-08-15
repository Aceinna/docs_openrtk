CAN J1939 Set Request Messages
*******************************

.. contents:: Contents
    :local:

.. toctree::
    :maxdepth: 1

**Set Commands**


The following Set requests have been implemented in the Example J1939 based Applications.  All Set requests are formed as a
Request message as specified earlier. The user can modify the provided requests and/or implement unique requests.

.. table::  *Set Commands*
    :align: left

    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    | **Request**               || **PF** || **PS**|| **PGN**     || **Payload** |  **Purpose**                            |
    |                           || (dec)  || (dec) ||             || **Length**  |                                         |
    |                           ||        ||       ||             || (bytes)     |                                         |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    | *Save Configuration*      |  255    | 81     |  65361       |  3           || Save all configuration                 |
    |                           |         |        |              |              || data to non-volatile memory            |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    | *Reset Algorithm*         |  255    | 80     |  65360       |  3           |  Reset algorithm to initial conditions  |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    | *Mag Alignment*           |  255    | 94     |  65374       |  2           |  Mag alignment and status request       |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    | *Set Packet Rate Divider* |  255    | 85     |  65365       |  2           || Set rate dividers to increase/decrease |
    |                           |         |        |              |              || rate packets are set                   |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    | *Set Data Packet Type(s)* |  255    | 86     |  65366       |  2           || Select 1, 2, or 3 packet types to send |
    |                           |         |        |              |              || periodically                           |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    || *Set Digital Filters*    |  255    | 87     |  65367       |  3           || Set cutoff frequency for rate and      |
    || *Cutoff Frequencies*     |         |        |              |              || accelerometers                         |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    | *Set Orientation*         |  255    | 88     |  65368       |  3           |  Set orientation of X, Y, and Z axes    |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    || *Set Bank of PS*         |  255    | 240    |  65520       |  8           || Sets PS number for the Reset Algorithm |
    || *Numbers for Bank0*      |         |        |              |              || Set request                            |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    || *Set Bank of PS*         |  255    | 241    |  65521       |  8           | Sets PS numbers for the other           |
    || *Numbers for Bank1*      |         |        |              |              | Set requests                            |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
                                                 
.. note::

    Provided PS values for all but the "Set Bank of PS Numbers for Bank0/Bank1" Set Commands can be changed by
    the "Set Bank of PS Numbers for Bank0/Bank1" message  The given values are the pre-defined values.



*Save Configuration*

    The next table provides the descriptions of the payload fields of the
    command and response messages.

    .. table::  *Save Configuration Request/Response Payload Fields*
        :align: left

        +----------+---------------------------------+
        | **Byte** | **Description/Values**          |
        +----------+---------------------------------+
        | 0        | Type: 0 = Request, 1 = Response |
        +----------+---------------------------------+
        | 1        | Destination Address             |
        +----------+---------------------------------+
        | 2        | Response: 0 = Fail, 1 = Succeed |
        +----------+---------------------------------+


*Reset Algorithm*

    The following table provides the descriptions of the payload fields of the
    command and response messages.

    .. table::  *Reset Algorithm Request/Response Payload Fields*
        :align: left

        +----------+---------------------------------+
        | **Byte** | **Description/Values**          |
        +----------+---------------------------------+
        | 0        | Type: 0 = Request, 1 = Response |
        +----------+---------------------------------+
        | 1        | Destination Address             |
        +----------+---------------------------------+
        | 2        | Response: 0 = Fail, 1 = Succeed |
        +----------+---------------------------------+


*Mag Alignment*

    The following tables provides the descriptions of the payload fields of the
    command and response messages.

    .. table::  *Mag Alignment Request Payload Fields*
        :align: left

        +----------+---------------------------------+
        | **Byte** | **Description/Values**          |
        +----------+---------------------------------+
        | 0        | Destination Address             |
        +----------+---------------------------------+
        | 1        | Command: 1 = Start, 0 - status  |
        +----------+---------------------------------+

    .. table::    *Payload Fields of 64 bit response*
        :align: left

        +--------------+-------------------------+-------------------------------------------------------+
        |   **Bits**   |   **Description**       |  Value                                                |
        +--------------+-------------------------+-------------------------------------------------------+
        ||  bits 0:7   || Command                || 0 - Status request,                                  |
        ||             ||                        || 1 - Start alignment                                  |
        +--------------+-------------------------+-------------------------------------------------------+
        ||  bit  8:15  || Alignment State        || 0  - Idle                                            |
        ||             ||                        || 12 - Alignment in process                            |
        ||             ||                        || 13 - Alignment complete                              |
        +--------------+-------------------------+-------------------------------------------------------+
        ||  bit  16:27 || Hard Iron X Bias, Gauss|| -8 G to +8 G , scale 1/256 G/bit, offset -8G         |
        +--------------+-------------------------+-------------------------------------------------------+
        ||  bits 28:39 || Hard Iron Y Bias, Gauss|| -8 G to +8 G , scale 1/256 G/bit, offset -8G         |
        +--------------+-------------------------+-------------------------------------------------------+
        ||  bits 40:49 || Soft Iron Ratio        || 0 to 1   1/1024 per Lsb                              |
        +--------------+-------------------------+-------------------------------------------------------+
        ||  bits 50:63 || Soft Iron Angle        || -3.14 to 3.14 RAD, scale 0.0015339, offset -3.14159  |
        +--------------+-------------------------+-------------------------------------------------------+


*Set Packet Rate Divider*

    The following table provides the values of the packet rate divider response payload

    .. table::  Set *Packet Rate Divider Request/Response Payload Fields*
        :align: left

        +--------+---------------+----------------------------------+
        | *Byte* | *Description* | **Byte Value**                   |
        +--------+---------------+----------------------------------+
        | 0      || Destination  | Unique                           |
        |        || Address      |                                  |
        +--------+---------------+----------------------------------+
        | 1      |               || **Byte Value** -                |
        |        |               | **Packet Broadcast Rate (Hz)**   |
        |        |               || 0  - Quiet Mode - no broadcast  |
        |        |  Packet       || 1  - 100 (default)              |
        |        |  Divider      || 2  - 50                         |
        |        |  Value        || 4  - 25                         |
        |        |               || 5  - 20                         |
        |        |               || 10 - 10                         |
        |        |               || 20 -  5                         |
        |        |               || 25 -  4                         |
        |        |               || 50 -  2                         |
        +--------+---------------+----------------+-----------------+

*Set Data Packet Type(s)*

    The following table provides the *Set Data Packet Type(s)* payload.  The 3 bits in the "data packet types"
    are bits in a bitmask that can be combined to use any 1 type, any 2 types, or all 3 types

    .. table::  *Set Data Packet Type(s) Field*
        :align: left


        +--------+----------------+-------------------------------+
        | *Byte* | *Description*  | **Byte Value**                |
        +--------+----------------+-------------------------------+
        | 0      |  Destination   | Unique                        |
        |        |  Address       |                               |
        +--------+----------------+-------------------------------+
        | 1      || Selected Data || Data Packet Type(s) Bitmask  |
        |        || Packet Type(s)|| Bit 0 - SSI2                 |
        |        || Bitmask (LSB) || Bit 1 - Angular Rate         |
        |        ||               || Bit 2 - Acceleration         |
        +--------+----------------+-------------------------------+
        | 2      || Selected Data ||  Reserved                    |
        |        || Packet Type(s)||                              |
        |        || Bitmask (MSB) ||                              |
        +--------+----------------+-------------------------------+

*Set Digital Filter Cutoff Frequencies*

    The following table shows provides descriptions of the response payload

    .. table::  *Digital Filter Cutoff Frequencies Request Payload*
        :align: left

        +--------------+------------------------------+----------------------------+
        || **Payload** | **Description/Values**       | **Values**                 |
        || **Byte**    |                              |                            |
        +--------------+------------------------------+----------------------------+
        | 0            | Destination Address          | Unique                     |
        +--------------+------------------------------+----------------------------+
        | 1            || Cutoff Frequencies (Hz) for |                            |
        |              || Angular Rate Sensors        | 0, 2, 5, 10, 25, 40, or 50 |
        +--------------+------------------------------+----------------------------+
        | 2            || Cutoff Frequencies (Hz) for |                            |
        |              || Accelerometer Sensors       | 0, 2, 5, 10, 25, 40, or 50 |
        +--------------+------------------------------+----------------------------+


*Set Orientation*

    The following table shows the payload layout

    .. table:: *Set Orientation Payload Layout*
        :align: left

        +----------+---------------------+-------------------+
        | **Byte** | **Meaning**         | **Value**         |
        +----------+---------------------+-------------------+
        | 0        | Destination Address | Unique            |
        +----------+---------------------+-------------------+
        | 1:2      | Orientation Setting || see table below  |
        |          |                     || LSB first        |
        +----------+---------------------+-------------------+

    The following table provides the values and meanings of the payload field bytes:

    .. table:: *Set Orientation Field Descriptions*
        :align: left

        +------------------+-----------------------+------------------+----------------+
        || **Orientation** | **X/Y/Z Axis**        || **Orientation** | **X/Y/Z Axis** |
        || **Field Value** |                       || **Field Value** |                |
        +------------------+-----------------------+------------------+----------------+
        | 0x0000           | +Ux +Uy +Uz (default) | 0x00C4           | +Uz +Uy -Ux    |
        +------------------+-----------------------+------------------+----------------+
        | 0x0009           | -Ux -Uy +Uz           | 0x00CD           | -Uz -Uy -Ux    |
        +------------------+-----------------------+------------------+----------------+
        | 0x0023           | -Uy +Ux +Uz           | 0x00D3           | -Uy +Uz -Ux    |
        +------------------+-----------------------+------------------+----------------+
        | 0x002A           | +Uy -Ux +Uz           | 0x00DA           | +Uy -Uz -Ux    |
        +------------------+-----------------------+------------------+----------------+
        | 0x0041           | -Ux +Uy -Uz           | 0x0111           | -Ux +Uz +Uy    |
        +------------------+-----------------------+------------------+----------------+
        | 0x0048           | +Ux -Uy -Uz           | 0x0118           | +Ux -Uz +Uy    |
        +------------------+-----------------------+------------------+----------------+
        | 0x0062           | +Uy +Ux -Uz           | 0x0124           | +Uz +Ux +Uy    |
        +------------------+-----------------------+------------------+----------------+
        | 0x006B           | -Uy -Ux -Uz           | 0x012D           | -Uz -Ux +Uy    |
        +------------------+-----------------------+------------------+----------------+
        | 0x0085           | -Uz +Uy +Ux           | 0x0150           | +Ux +Uz -Uy    |
        +------------------+-----------------------+------------------+----------------+
        | 0x008C           | +Uz -Uy +Ux           | 0x0159           | -Ux -Uz -Uy    |
        +------------------+-----------------------+------------------+----------------+
        | 0x0092           | +Uy +Uz +Ux           | 0x0165           | -Uz +Ux -Uy    |
        +------------------+-----------------------+------------------+----------------+
        | 0x009B           | -Uy -Uz +Ux           | 0x016C           | +Uz -Ux -Uy    |
        +------------------+-----------------------+------------------+----------------+


*Set Bank of PS Numbers*

    The following tables provide descriptions of the payload for Bank0 and Bank1 set commands

    .. table:: *Set Bank of PS Numbers for Bank0 Payload*
        :align: left

        +----------+-------------------------------+
        | **Byte** | **Parameters**                |
        +----------+-------------------------------+
        | 0        | Reset Algorithm PS number     |
        +----------+-------------------------------+
        | 2-7      | Reserved                      |
        +----------+-------------------------------+

    .. table::  *Set Bank of PS Numbers for Bank1 Payload*
        :align: left

        +----------+------------------------------------------------+
        | **Byte** | **Parameters**                                 |
        +----------+------------------------------------------------+
        | 0        | Set Packet Rate PS number                      |
        +----------+------------------------------------------------+
        | 1        | Set Packet Type(s) PS number                   |
        +----------+------------------------------------------------+
        | 2        | Set Digital Filer Cutoff Frequencies PS number |
        +----------+------------------------------------------------+
        | 3        | Set Orientation PS Number                      |
        +----------+------------------------------------------------+
        | 4        | Set User Behavior PS Number                    |
        +----------+------------------------------------------------+
        | 5        | Set Angle Alarm PS Number                      |
        +----------+------------------------------------------------+
        | 6        | Set Cone Alarm PS Number                       |
        +----------+------------------------------------------------+
        | 7        | Set Acceleration PS Number                     |
        +----------+------------------------------------------------+
