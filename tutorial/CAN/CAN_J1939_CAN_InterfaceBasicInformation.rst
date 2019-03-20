J1939 CAN Interface Basic Information
*************************************

.. contents:: Contents
    :local:
    
Basic details of the J1939 CAN Interface are explained first because 
description of the J1939 CAN example application depends heavily on prior knowledge of  
these basic details:

J1939 CAN Message Characteristics
---------------------------------

*   All multiple byte fields are sent LSB first, then more significant byte(s) of the 
    field in low to high byte order.
.. 
*   All messages, with the exception of the Request Message, have a standard 29 bit header, defined in the CAN 2.0B 
    protocol.  This 29-bit header is also known as the *ECU Identifier":
..

    .. table:: **Header - ECU Identifier Layout**
        :align: left

        +-----------+--------------------------+-------------+
        | **Byte**  | **Contents**             | **Acronym** |
        +-----------+--------------------------+-------------+
        || Preamble || bits 0:2 - Priority     || Prio       |
        || Bits     || bit  3   - Reserved Bit || R          |
        |           || bit  4   - Data Page    || DP         |
        +-----------+--------------------------+------+------+
        | 0         | PDU Format               | PF   |      |
        +-----------+--------------------------+------+ PGN  |
        | 1         | PDU Specific             | PS   |      |
        +-----------+--------------------------+------+------+
        | 2         | Source Address           | SA          |
        +-----------+--------------------------+-------------+
..

    *   *Header - ECU Identifier* field descriptions

        *   Preamble Bits

            *   Priority bits - 0 to 7, 0 is highest
            *   Reserved bit - set to 0 on transmit
            *   Data Page bit - J1939 currently uses 0 to indicate that the 29 bit identifier is used.
            *   The Priority, Reserved Bit (transmitted as 0), and the DP bit are transmitted first as 5 bits.  The example application
                stores the Priority and ignores the R Bit and the DP bit.

        *   Bytes 0 and 1

            *   PDU Format (PF) & PDU Specific (PS) (AKA PGN)

                *   PF between 0 and 239 - PS field contains specific destination Address
                *   PF between 240 and 255 - The message is a broadcast message.  The PS field 
                    contains a Group Extension, expands the number of possible broadcast 
                    Parameter Groups that can be represented by the identifier.

        *   Byte 2

            *   Source Address.  The address of the sender of the message.

CAN Bus Address Arbitration
-----------------------------

        Every ECU on a CAN network has to have a unique ECU address.  The example application
        provides a CAN network address arbitration scheme.  

Baud Rate
-----------

        The J1939 network default baud rate of J1939 is 250Kbps. The OpenIMU300RI also supports a lower speed and a higher speed 
        (125Kbps and 500Kbps). 

ECU Name
----------

        The "*Name*" is a 64 bit (8 bytes) long label which gives every ECU a unique identity. 
        The main purpose of the Name is to describe the ECU for other ECUs.  It is 8 bytes long 
        and is composed of 10 fields with the layout and structure shown in the following tables.  
        In the table, the acronyms "LO" and "HO" mean "Low Order" and "High Order".

        .. table:: **Name Layout**
            :align: left

            +---------------------+-------------------------------------+
            || **Byte number in** || **Contents**                       |
            || **CAN Message**    |                                     |
            +---------------------+-------------------------------------+
            | 0                   |  LSB of Identity Number             |
            +---------------------+-------------------------------------+
            | 1                   || 2 middle bytes of                  |
            |                     || Identity Number (IDN)              |
            |                     || Lower order byte first             |
            +---------------------+-------------------------------------+
            | 2                   || Bits 0:4 - HO Nibble of IDN        |
            |                     || Bits 5:7 - LO Nibble Mfgr Code     |
            +---------------------+-------------------------------------+
            | 3                   || MSB Mfgr Code                      |
            +---------------------+-------------------------------------+
            | 4                   || Bits 0:2 - ECU Instance            |
            |                     || Bits 3:7 - Function Instance       |
            +---------------------+-------------------------------------+
            | 5                   |  Function                           |
            +---------------------+-------------------------------------+
            | 6                   || Bit  0   - Reserved                |
            |                     || Bits 1:7 - Vehicle System          |
            +---------------------+-------------------------------------+
            | 7                   || Bits 0:3 - Vehicle system instance |
            |                     || Bits 4:6 - Industry Group          |
            |                     || Bit  7   - Arbitrary Address Bit   |
            +---------------------+-------------------------------------+

        .. table:: **Name Structure**

            +-------------------------+--------------+
            | **Field**               || **Number**  |
            |                         || **of bits** |
            +-------------------------+--------------+
            | Identity number         | 21           |
            +-------------------------+--------------+
            | Manufacturer code       | 11           |
            +-------------------------+--------------+
            | ECU instance            |  3           |
            +-------------------------+--------------+
            | Function instance       |  5           |
            +-------------------------+--------------+
            | Function                |  8           |
            +-------------------------+--------------+
            | Reserved bit            |  1           |
            +-------------------------+--------------+
            | Vehicle system          |  7           |
            +-------------------------+--------------+
            | Vehicle system instance |  4           |
            +-------------------------+--------------+
            | Industry group          |  3           |
            +-------------------------+--------------+
            | Arbitrary address bit   |  1           |
            +-------------------------+--------------+


.. note:: Use the "CAN J1939 Example Application" Link on the left to follow the flow for the J1939 CAN Application.

