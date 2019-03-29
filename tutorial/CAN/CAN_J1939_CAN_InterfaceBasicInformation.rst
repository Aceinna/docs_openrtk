J1939 CAN Interface Basic Information
*************************************

.. contents:: Contents
    :local:

Basic details of the J1939 CAN Interface as implemented in the example application are explained first to
facilitate understanding of the J1939 CAN example application


.. note::
    **Byte Order - All multiple byte fields are sent LSB first, then more significant byte(s) of the
    field in lesser to higher byte significance**.

**CAN Bus Address Arbitration**

        Every ECU on a CAN network has to have a unique ECU address.  The example application
        provides a CAN network address arbitration scheme.  Refer to the explanation of the "*Address Claiming* State
        of the *CAN Communication Task* in a later page.

**Baud Rate**

    The J1939 network default baud rate of J1939 is 250Kbps. The OpenIMU300RI supports 125Kbps, 500Kbps, 500kbps, and 1Mbps.

**Messages, Data Packets, and CAN Parameter Groups**

    CAN documentation uses the term "Parameter Group", which is synonymous with the term "message" in this documentation.  The requested data messages sent on a periodic basis are referred to as "data packets".

**Message Header - ECU Identifier**

    All CAN messages, with the exception of the Request Parameter Group, have a 29 bit header, which is also referred to as the *ECU Identifier*.
    The Request Parameter Group (RQST) is described below.

    .. table:: **Header - ECU Identifier Layout**
        :align: left

        +-----------+--------------------------------+-------------+
        | **Byte**  | **Contents**                   | **Acronym** |
        +-----------+--------------------------------+-------------+
        || Preamble || bits 0:2 - Priority           || Prio       |
        || Bits     || bit  3   - Extended Data Page || EDP        |
        |           || bit  4   - Data Page          || DP         |
        +-----------+--------------------------------+-------------+
        | 0         | PDU Format                     | PF          |
        +-----------+--------------------------------+-------------+
        | 1         | PDU Specific                   | PS          |
        +-----------+--------------------------------+-------------+
        | 2         | Source Address                 | SA          |
        +-----------+--------------------------------+-------------+

**Parameter Group Number (PGN)**

    The Parameter Group Number is an 18 bit number composed as follows:

    *   2 bits - EDP & DP bits
    *   8 bits - PF
    *   8 bits - PS

**Type of Parameter Groups**

    *   *Global Parameter Groups*.  Global PGNs identify parameter groups that are globally broadcast.  The PF, PS, DP and EDP are
        set to identify a Parameter Group rather than a specific address.   Global Parameter Groups have a PF that is 240 or greater and a PS
        that identifies a Group Extension.
    *   *Specific Parameter Groups*.  For Specific Parameter Groups, the PF value is the address of a specific CAN node and the PS is 0.

**Special Parameter Groups**

    *Request Parameter Group*

        The request parameter group, aka RQST (PGN 0x00EA0016) can be sent to all or a specific CA to request a specified parameter group.

        The RQST contains the PGN of the request parameter group. If the receiver of a specific request cannot respond,
        it must send a negative acknowledgment.

        The RQST has a data length code of 3 bytes and is the only parameter group with a data length code less than 8 bytes.

    *Acknowledgement (ACK) Parameter Group*

        The ACK Parameter Group (PGN 0x00E800) is used to send a positive or negative acknowledgement to a request.

    *Address claiming parameter group*

        The address claiming parameter group (PGN 0x00EE0016) is used to arbitrate CAN addresses.

    *Commanded address parameter group*

        The commanded address parameter group (PGN 0x00FED816) can be used to change the address of a CAN node.

    *Transport protocol parameter group*

        The transport protocol parameter groups (PGN 0x0EC0016 and 0x00EB0016) are used to packetize and transfer
        parameter groups with more than 8 data bytes.


**ECU Name**

        The "*Name*" is a 64 bit (8 bytes) long label which gives every ECU a unique identity.
        The main purpose of the Name is to describe the ECU for other ECUs.  It is 8 bytes long
        and is composed of 10 fields with the layout and structure shown in the following tables.
        In the table, the acronyms "LO" and "HO" mean "Low Order" and "High Order".

        .. table:: **Name Layout**
            :align: left

            +---------------------+-------------------------------------+
            || **Byte number in** |  **Contents**                       |
            || **CAN Message**    |                                     |
            +---------------------+-------------------------------------+
            | 0                   |  LSB of Identity Number (IDN)       |
            +---------------------+-------------------------------------+
            | 1                   |  IDN - middle byte                  |
            +---------------------+-------------------------------------+
            | 2                   || Bits 0:4 - HO Nibble of IDN        |
            |                     || Bits 5:7 - LO Nibble Mfgr Code     |
            +---------------------+-------------------------------------+
            | 3                   |  MSB Mfgr Code                      |
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
