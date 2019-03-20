CAN J1939 Set Command Messages
*******************************

.. contents:: Contents
    :local:
    
.. toctree::
    :maxdepth: 1
    
Set Commands
-------------

The following Set commands have been implemented in the Example Application.  
You can modify the provided commands and/or implement unique commands.  

.. table::  *Set Commands*
    :align: left

    +--------------------------+---------+---------------------+-----------------------------------------------+
    | **Command**              | **PGN** || **Payload**        || **Purpose**                                  |
    |                          |         || **Length**         ||                                              |
    |                          |         || (bytes)            ||                                              |
    +--------------------------+---------+---------------------+-----------------------------------------------+
    | *Save Configuration*     || 65361  || 3                  || Save all configuration                       |
    |                          ||        ||                    || data to non-volatile memory                  |
    +--------------------------+---------+---------------------+-----------------------------------------------+
    | *Reset Algorithm*        |  65360  |  3                  |  Reset algorithm to initial conditions        |
    +--------------------------+---------+---------------------+-----------------------------------------------+
    | *Set PacketRate Divider* || 65365  || 2                  || Set rate dividers to increase/decrease       |
    |                          ||        ||                    || rate packets are set                         |
    +--------------------------+---------+---------------------+-----------------------------------------------+
    | *Set Data Packet Type(s)*|| 65366  || 2                  || Select 1, 2, or 3 packet types to send       |
    |                          ||        ||                    || periodically                                 |
    +--------------------------+---------+---------------------+-----------------------------------------------+
    || *Set Digital Filters*   || 65367  || 3                  || Set cutoff frequency for rate and            |
    || *Cutoff Frequencies*    ||        ||                    || accelerometers                               |
    +--------------------------+---------+---------------------+-----------------------------------------------+
    | *Set Orientation*        |  65368  |  3                  |  Set orientation of X, Y, and Z axes          |
    +--------------------------+---------+---------------------+-----------------------------------------------+
    || *Set Bank of PS*        || 65520  || 8                  || Sets PS numbers for the 3 Get commands       |
    || *Numbers for Bank0*     ||        ||                    || and the Reset Algorithm Set command          |
    +--------------------------+---------+---------------------+-----------------------------------------------+
    || *Set Bank of PS*        || 65521  || 8                  |  Sets PS numbers for the 4 other Set commands |
    || *Numbers for Bank1*     ||        ||                    |                                               |
    +--------------------------+---------+---------------------+-----------------------------------------------+



*Save Configuration* Set Command
--------------------------------

The next table provides the descriptions of the payload fields of the 
command and response messages.

.. table::  *Save Configuration Command/Response Payload Fields*
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


*Reset Algorithm* Set Command
-----------------------------

The next table provides the descriptions of the payload fields of the 
command and response messages.

.. table::  *Reset Algorithm Command/Response Payload Fields*
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


*Set Packet Rate Divider* Set Command
-------------------------------------

The following table provides the values of the packet rate divider payload byte.

.. table::  Set *Packet Rate Divider Command/Response Payload Fields*
    :align: left

    +--------+---------------+----------------------------------+
    | *Byte* | *Description* | **Byte Value**                   |
    +--------+---------------+----------------------------------+
    | 0      || Destination  | Unique                           |
    |        || Address      |                                  |
    +--------+---------------+----------------------------------+
    | 1      ||              || **Byte Value** -                |
    |        ||              || **Packet Broadcast Rate (Hz)**  |
    |        ||              || 0  - Quiet Mode - no broadcast  |
    |        || Packet       || 1  - 100 (default)              |
    |        || Divider      || 2  - 50                         |
    |        || Value        || 4  - 25                         |
    |        ||              || 5  - 20                         |
    |        ||              || 10 - 10                         |
    |        ||              || 20 -  5                         |
    |        ||              || 25 -  4                         |
    |        ||              || 50 -  2                         |
    +--------+---------------+----------------+-----------------+

*Set Data Packet Type(s)* Set Command
-------------------------------------

The following table provides the payload byte field.

.. table::  *Set Data Packet Type(s) Field*
    :align: left


    +--------+---------------+----------------------------------+
    | *Byte* | *Description* | **Byte Value**                   |
    +--------+---------------+----------------------------------+
    | 0      || Destination  | Unique                           |
    |        || Address      |                                  |
    +--------+---------------+----------------------------------+
    | 1      || Selected     || Value - Data                    |
    |        || Data         || Packet Type(s)                  |
    |        || Packet       || 1 - SSI2                        |
    |        || Type(s)      || 2 - Angular Rate                |
    |        ||              || 3 - SSI2 & Angular Rate         |
    |        ||              || 4 - Acceleration                |
    |        ||              || 5 - SSI2 & Acceleration         |
    |        ||              || 6 - Angular Rate & Acceleration |
    |        ||              || 7 - All 3                       |
    +--------+---------------+----------------------------------+

*Set Digital Filter Cutoff Frequencies* Set Command
---------------------------------------------------

The following table shows provides descriptions of the command payload 
of the payload bytes.


.. table::  *Digital Filter Cutoff Frequencies Command Payload*
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


*Set Orientation* Set Command
-----------------------------

The following table shows the payload layout

.. table:: *Set Orientation Payload Layout*
    :align: left

    +----------+---------------------+------------------+
    | **Byte** | **Meaning**         | **Value**        |
    +----------+---------------------+------------------+
    | 0        | Destination Address | Unique           |
    +----------+---------------------+------------------+
    | 1        | Orientation Setting | see table below  |
    +----------+---------------------+------------------+


The following table provides the values and meanings of the payload field bytes:

.. table:: *Set Orientation Field Descriptions*
    :align: left

    +------------------+-----------------------+
    || **Orientation** | **X/Y/Z Axis**        |
    || **Field Value** |                       |
    +------------------+-----------------------+
    | 0x0000           | +Ux +Uy +Uz (default) |
    +------------------+-----------------------+
    | 0x0009           | -Ux -Uy +Uz           |
    +------------------+-----------------------+
    | 0x0023           | -Uy +Ux +Uz           |
    +------------------+-----------------------+
    | 0x002A           | +Uy -Ux +Uz           |
    +------------------+-----------------------+
    | 0x0041           | -Ux +Uy -Uz           |
    +------------------+-----------------------+
    | 0x0048           | +Ux -Uy -Uz           |
    +------------------+-----------------------+
    | 0x0062           | +Uy +Ux -Uz           |
    +------------------+-----------------------+
    | 0x006B           | -Uy -Ux -Uz           |
    +------------------+-----------------------+
    | 0x0085           | -Uz +Uy +Ux           |
    +------------------+-----------------------+
    | 0x008C           | +Uz -Uy +Ux           |
    +------------------+-----------------------+
    | 0x0092           | +Uy +Uz +Ux           |
    +------------------+-----------------------+
    | 0x009B           | -Uy -Uz +Ux           |
    +------------------+-----------------------+
    | 0x00C4           | +Uz +Uy -Ux           |
    +------------------+-----------------------+
    | 0x00CD           | -Uz -Uy -Ux           |
    +------------------+-----------------------+
    | 0x00D3           | -Uy +Uz -Ux           |
    +------------------+-----------------------+
    | 0x00DA           | +Uy -Uz -Ux           |
    +------------------+-----------------------+
    | 0x0111           | -Ux +Uz +Uy           |
    +------------------+-----------------------+
    | 0x0118           | +Ux -Uz +Uy           |
    +------------------+-----------------------+
    | 0x0124           | +Uz +Ux +Uy           |
    +------------------+-----------------------+
    | 0x012D           | -Uz -Ux +Uy           |
    +------------------+-----------------------+
    | 0x0150           | +Ux +Uz -Uy           |
    +------------------+-----------------------+
    | 0x0159           | -Ux -Uz -Uy           |
    +------------------+-----------------------+
    | 0x0165           | -Uz +Ux -Uy           |
    +------------------+-----------------------+
    | 0x016C           | +Uz -Ux -Uy           |
    +------------------+-----------------------+

*Set Bank of PS Numbers* Set Command
------------------------------------
The following tables provide descriptions of the command payload 
for Bank0 and Bank1

.. table::  *Set Bank of PS Numbers for Bank0 Command Payload Fields*
    :align: left

    +----------+--------------------------------+
    | **Byte** | **Description/Values**         |
    +----------+--------------------------------+
    | 0        | Destination Address            |
    +----------+--------------------------------+
    | 1        || Set Reset Algorithm PS number |
    +----------+--------------------------------+
    | 2        || Get Hardware BIT PS number    |
    +----------+--------------------------------+
    | 3        || Get Software BIT PS number    |
    +----------+--------------------------------+
    | 4        || Get Status PS Number          |
    +----------+--------------------------------+
    | 5-9      || Reserved                      |
    +----------+--------------------------------+

.. table::  *Set Bank of PS Numbers for Bank1 Command Payload Fields*
    :align: left

    +----------+-------------------------------------------------+
    | **Byte** | **Description/Values**                          |
    +----------+-------------------------------------------------+
    | 0        | Destination Address                             |
    +----------+-------------------------------------------------+
    | 1        || Set Packet Rate PS number                      |
    +----------+-------------------------------------------------+
    | 2        || Set Packet Type(s) PS number                   |
    +----------+-------------------------------------------------+
    | 3        || Set Digital Filer Cutoff Frequencies PS number |
    +----------+-------------------------------------------------+
    | 4        || Set Orientation PS Number                      |
    +----------+-------------------------------------------------+
    | 5-9      || Reserved                                       |
    +----------+-------------------------------------------------+


