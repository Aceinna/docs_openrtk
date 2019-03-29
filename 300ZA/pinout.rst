Connector Pinout - Including GPS Sensor Interface
=================================================

.. contents:: Contents
    :local:

The OpenIMU300ZA main connector is a SAMTEC FTM-110-02-F-DV-P defined below. The mating connector that pairs with the main connector is the SAMTEC CLM-110-02.

|Connector.png|

                   OpenIMU300ZA Interface Connector

J2 is 20-pin connector and it used for connecting the OpenIMU300ZA unit into Open IMU evaluation board.  The connector pin definitions are defined in the table below.  The GPS-related signals are noted.

**Table Interface Connector Pin Definition**


    +-----------------+-------------------------+-----------------------+
    | **Pin**         |   Main Function         | Alternative Function  |
    |                 |                         |                       |
    +-----------------+-------------------------+-----------------------+
    || 1              || Output. Inertial-Sensor|| Can be used as GPIO  |
    ||                |  Sampling Indicator     | (IO3)                 |
    ||                || (sampling upon         |                       |
    ||                |  falling edge)          |                       |
    +-----------------+-------------------------+-----------------------+
    | 2               || Synchronization Input - 1PPS Input             |
    +-----------------+-------------------------+-----------------------+
    | 3               || User UART TX  (Output) || SPI Clock (SCLK)     |
    |                 |                         || (Output)             |
    +-----------------+-------------------------+-----------------------+
    | 4               | User UART RX  (Input)   | SPI Data Output       |
    |                 |                         | (MISO)                |
    +-----------------+-------------------------+-----------------------+
    | 5               | GPS UART TX (Output)    | SPI Data Input (MOSI))|
    +-----------------+-------------------------+-----------------------+
    | 6               | GPS UART RX  (Input)    | SPI Chip Select (SS)  |
    +-----------------+-------------------------+-----------------------+
    | 7               || Data Ready (SPI        || SPI/UART Interface   |
    |                 || Communication Data)    || Selector             |
    +-----------------+-------------------------+-----------------------+
    | 8               |             External Reset (NRST))              |
    +-----------------+-------------------------+-----------------------+
    | 9               | GPIO Output             || Can be used as GPI0  |
    |                 |                         || (IO2)                |
    +-----------------+-------------------------+-----------------------+
    | 10              | Power VIN (3-5 VDC)     | Power VIN (3-5 VDC)   |
    +-----------------+-------------------------+-----------------------+
    | 11              | Power VIN (3-5 VDC)     | Power VIN (3-5 VDC)   |
    +-----------------+-------------------------+-----------------------+
    | 12              | Power VIN (3-5 VDC)     | Power VIN (3-5 VDC)   |
    +-----------------+-------------------------+-----------------------+
    | 13              | Power GND               | Power GND             |
    +-----------------+-------------------------+-----------------------+
    | 14              | Power GND               | Power GND             |
    +-----------------+-------------------------+-----------------------+
    | 15              | Power GND               | Power GND             |
    +-----------------+-------------------------+-----------------------+
    | 16              | SWDIO (SWD debug interface)                     |
    +-----------------+-------------------------+-----------------------+
    | 17              | External GPS UART TX    |Debug interface UART TX|
    +-----------------+-------------------------+-----------------------+
    | 18              | SWCLK (SWD debug interface)                     |
    +-----------------+-------------------------+-----------------------+
    | 19              | External GPS UART RX    |Debug Interface UART RX|
    +-----------------+-------------------------+-----------------------+
    | 20              | Reference voltage for SWD debug interface       |
    +-----------------+-------------------------+-----------------------+

**Power Input and Power Input Ground**

Power is applied to the OpenIMU300ZA on pins 10 through 15. Pins 13-15 are
ground; Pins 10-12 accepts 3 to 5 VDC unregulated input. Note that these
are redundant power ground input pairs.

.. note::

    Do not reverse the power leads or damage may occur. Do not add greater
    than 5.5 volts on the power pins or damage may occur. This system has no
    reverse voltage or over-voltage protection.

.. |Connector.png| image:: ../media/image2.png
   :width: 2.24in
   :height: 2.85in
