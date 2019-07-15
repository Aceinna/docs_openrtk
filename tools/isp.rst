In-System Update
================

All OpenIMU hardware modules come shipped pre-configured with a special
bootloader resident in their FLASH memory. This bootloader allows for
in-system code updates using a UART connection without using JTAG.  Sample code that utilizes this
Bootloader can be found in the OpenIMU Python driver.  An example of how to invoke the Python driver
for code loading is here.

The full details of the bootloader serial protocol is described below.  These commands are executed
using OpenIMU's standard serial interface:

**Bootloader Initialization**

    A user can initiate bootloader at any time by sending ‘JI’ command
    (see below command’s format) to application program. This command
    forces the unit to enter bootloader mode.  The unit will communicate
    at 57.6Kbps baud rate regardless of the original baud rate the unit
    is configured to. The Bootloader always communicates at 57.6Kbps
    until the firmware upgrade is complete.

    As an additional device recovery option immediately after powering
    up, every OpenIMU will enter a recovery window of 100ms prior to
    application start.  During this 100mS window, the user can send
    ‘JI’ command at 57.6Kbs to the Bootloader in order to force the
    unit to remain in Bootloader mode.

    Once the device enters Bootloader mode via the ‘JI’ command either
    during recovery window or from normal operation, a user can send
    a sequence of ‘WA’ commands to write a complete application image
    into the device’s FLASH.

    After loading the entire firmware image with successive ‘WA’
    commands, a ‘JA’ command is sent to instruct the unit to exit
    Bootloader mode and begin application execution.  At this point
    the device will return to its original baud rate.

    Optionally, the system can be rebooted by toggling the power or toggling
    nRst (pull low and release) to restart the system.

**Firmware Update Commands**

    The commands detailed below are used for
    upgrading a new firmware version via the UART at 57.6Kbps.

    **Jump to Bootloader Command**


        +---------------------------------------------------------------------+
        | **Jump To Bootloader ('JI'=0x4A49)**                                |
        +----------+-------------+--------+---------+-------------------------+
        | Preamble | Packet Type | Length | Payload | Termination             |
        +----------+-------------+--------+---------+-------------------------+
        | 0x5555   | 0x4A49      | 0x00   |         | CRC(U2)                 |
        +----------+-------------+--------+---------+-------------------------+

        The command allows system to enter bootloader mode.

    **Write App Command**


        +---------------------------------------------------------------------+
        | **Write APP ('WA'=0x5741)**                                         |
        +----------+-------------+--------+---------+-------------------------+
        | Preamble | Packet Type | Length | Payload | Termination             |
        +----------+-------------+--------+---------+-------------------------+
        | 0x5555   | 0x5741      | len+5  |         | CRC(U2)                 |
        +----------+-------------+--------+---------+-------------------------+

        The command allows users to write binary sequentially to flash memory
        in bootloader mode. The total length is the sum of payload’s length and
        4-byte address followed by 1-byte data length. See the following table
        of the payload’s format.

        +---------------------------------------------------------------------+
        | **WA Payload Contents**                                             |
        +-------------+-------------+--------+---------+-------+--------------+
        | Byte Offset | Name        | Format | Scaling | Units | Description  |
        +-------------+-------------+--------+---------+-------+--------------+
        | 0           | staringAddr | U4     | -       | bytes || The FLASH   |
        |             |             |        |         |       | word offset  |
        |             |             |        |         |       || to begin    |
        |             |             |        |         |       | writing data |
        +-------------+-------------+--------+---------+-------+--------------+
        | 4           | byteLength  | U1     | -       | bytes || The word    |
        |             |             |        |         |       | length of the|
        |             |             |        |         |       || the data to |
        |             |             |        |         |       | write        |
        +-------------+-------------+--------+---------+-------+--------------+
        | 5           | dataByte0   | U1     | -       | -     | Flash data   |
        +-------------+-------------+--------+---------+-------+--------------+
        | 6           | dataByte1   | U1     | -       | -     | Falsh data   |
        +-------------+-------------+--------+---------+-------+--------------+
        | ...         | ...         |        |         |       |              |
        +-------------+-------------+--------+---------+-------+--------------+
        | 4+byteLength| dataByte    | U1     | -       | -     | Flash data   |
        +-------------+-------------+--------+---------+-------+--------------+

        Payload starts from 4-byte address of flash memory where the binary is
        located. The fifth byte is the number of bytes of dataBytess, but less
        than 240 bytes. User must truncate the binary to less than 240-byte blocks
        and fill each of blocks into payload starting from the sixth-byte. See
        the reference code, function write_block(), in Appendix F.

    **Jump to Application Command**


        +---------------------------------------------------------------------+
        | **Jump To Application ('JA"=0x4A41)**                               |
        +----------+-------------+--------+---------+-------------------------+
        | Preamble | Packet Type | Length | Payload | Termination             |
        +----------+-------------+--------+---------+-------------------------+
        | 0x5555   | 0x4A41      | 0x00   |         | CRC(U2)                 |
        +----------+-------------+--------+---------+-------------------------+

        The command allows system directly to enter application mode.

