Overview
==========

.. contents:: Contents
    :local:

**1. Introduction**
~~~~~~~~~~~~~~~~~~~~~

    The OpenRTK evaluation kit (EVK) is a hardware platform to evaluate the
    OpenRTK330 GNSS RTK/INS integrated positioning system and develop various
    applications based on this platform. Supported by the online Aceinna Navigation
    Studio the kit provides easy access to the features of OpenRTK330 and
    explains how to integrate the device in a custom design. The OpenRTK 
    EVK is shown below after unpacking.

    .. image:: ../media/EvalKit.png

where

  * 1: ST-Link debugger
  * 2: Multi-Constellation Multi-frequency GNSS antenna
  * 3: Micro-USB cable
  * 4: OpenRTK330 Evaluation Board (EVB) with metal flat mounting board
  * 5: 12-V DC adapter with 5.5 x 2.1 mm power jack

**2. OpenRTK330 EVB**
~~~~~~~~~~~~~~~~~~~~~~~

An OpenRTK330 Evaluation board is shown below in detail

 .. figure:: ../media/EvalBoard.png
    :width: 6.0in
    :height: 6.0in

where

  * 1: OpenRTK330 GNSS/IMU integrated module
  * 2: GNSS antenna SMA interface
  * 3: Espressif ESP32 bluetooth module
  * 4: SWD/JTAG connector, 20-pin
  * 5: Extension connector with 6-pin interfaces from left to right

      - GND
      - Not Connected
      - Not Connected
      - Connects to pin #56 "USER_UART2_TX" of the OpenRTK330 module
      - Connects to pin #55 "USER_UART2_RX" of the OpenRTK330 module
      - 1PPS outlet

  - 6. Extension connector with 6-pin SPI interfaces from left to right

      - Connects to pin #29 "USER_MOSI" of the OpenRTK330 module
      - Connects to pin #30 "USER_SCK" of the OpenRTK330 module
      - Connects to pin #31 "USER_NSS" of the OpenRTK330 module
      - Connects to pin #32 "USER_MISO" of the OpenRTK330 module
      - Connects to pin #39 "USER_DRDY" of the OpenRTK330 module
      - GND 

  - 7. Boot mode switch with two positions (A and B)
  - 8. RJ45 jack for Ethernet interface
  - 9. Micro-USB port
  - 10. CAN interface
  - 11. Power jack for 12-v adapter
  - 12. EVB working status LEDs from left to right 

    - Yellow: ST GNSS chipset is powered on and working properly
    - Red: valid GNSS base station data receiving
    - Green: valid GNSS signal receiving


.. **2.1 OpenRTK330 module**
..
     OpenRTK330 integrates a multi-constellation,
     multi-frequency Global Navigation Satellite System (**GNSS**) chipset
     (supports GPS, GALILEO, GLONASS and Beidou), triple-redundant
     6-axis (3-axis accelerometer and 3-axis gyro) **MEMS** Inertial
     Measurement Unit. It is used as the base for development custom RTK/INS
     applications.

.. **2.2 OpenRTK Evaluation board**
..
     OpenRTK Evaluation board designed to provide convenient way for
     communicating with OpenRTK330 unit from PC, mobile and car, to expose
     serial and SPI interfaces to developer and to debug applications using
     ST-Link debugger vis SWD interface.

.. **2.3 ST-Link debugger**
..
     St-Link debugger is standard debugger provided by STMicroelectronics
     company. It used for in-system debugging of applications via SWD
     interface.

.. **3. OpenRTK evaluation board Headers and Connectors**

.. **3.1 Connector for plugging in OpenRTK330 unit (U7)**
..
    It used for connecting the OpenRTK330 unit into OpenRTK evaluation
    board. The pin functions are described in the table on the “OpenRTK330
    Modules » OpenRTK330 - EZ Embed Automotive Module » Connector Pinout”
    page accessible from the Contents bar on the left.

.. 
    **3.2 Extension Header (J11/J12)**
..
    OpenRTK evaluation board has two extension headers. J11 has 12 pins and
    12 has 6 pins. It designed to expose RTK interface signals to external
    system. The extension header pin functions described in table below.
..
    **J11:**
..
    +-----------------+----------------------------+
    | **Pin**         |   Main Function            |
    +-----------------+----------------------------+
    | 1               | Power VDD 3V3              |
    +-----------------+----------------------------+
    | 2               | MOSI (SPI Data Input)      |
    +-----------------+----------------------------+
    | 3               | MISO (SPI Data Output)     |
    +-----------------+----------------------------+
    | 4               | SCK(SPI Clock Input)       |
    +-----------------+----------------------------+
    | 5               | NSS (SPI Chip Select Input)|
    +-----------------+----------------------------+
    | 6               | Power GND                  |
    +-----------------+----------------------------+
    | 7               | GPIO (LED3)                |
    +-----------------+----------------------------+
    | 8               | GPIO (LED2)                |
    +-----------------+----------------------------+
    | 9               | GPIO (LED1)                |
    +-----------------+----------------------------+
    | 10              | DRDY (Data Ready)          |
    +-----------------+----------------------------+
    | 11              | User UART RX (UART3 input) |
    +-----------------+----------------------------+
    | 12              | User UART TX (UART3 output)|
    +-----------------+----------------------------+
..
    J12:
..
    +-----------------+----------------------------+
    | **Pin**         |   Main Function            |
    +-----------------+----------------------------+
    | 1               | MOSI (SPI Data Input)      |
    +-----------------+----------------------------+
    | 2               | MISO (SPI Data Output)     |
    +-----------------+----------------------------+
    | 3               | SCK (SPI Clock Input)      |
    +-----------------+----------------------------+
    | 4               | NSS (SPI Chip Select Input)|
    +-----------------+----------------------------+
    | 5               | DRDY (Data Ready)          |
    +-----------------+----------------------------+
    | 6               | Power GND                  |
    +-----------------+----------------------------+
..
    **3.3 SWD (JTAG) connector (J10)**
..
    20-pin connector J10 used for connecting ST-Link or J-Link debuggers to the RTK for 
    in-system debugging of applications via SWD interface. It has standard pin-out.
..
    +-------------------+-------------------------+
    | **Pin**           |   Main Function         |
    |                   |                         |
    +-------------------+-------------------------+
    | 1                 | VDD 3V3                 |
    +-------------------+-------------------------+
    | 4, 6, 8, 10 , 12  | GND                     |
    | 14, 16, 18, 20    |                         |
    +-------------------+-------------------------+
    | 7                 | SWDIO                   |
    +-------------------+-------------------------+
    | 9                 | SWCLK                   |
    +-------------------+-------------------------+
    | 15                | nRST                    |
    +-------------------+-------------------------+
..
    **3.4 ESP32 UART (J4)**
..
    6-pin connector J4 used for connecting TTL USB to the ESP32. It can download ESP32 Firmware.
..	
    +-----------------+-----------------------------+
    | **Pin**         |   Main Function             |
    +-----------------+-----------------------------+
    | 1               | GND                         |
    +-----------------+-----------------------------+
    | 4               | ESP32 RX (MCU USER UART2 TX)|
    +-----------------+-----------------------------+
    | 5               | ESP32 TX (MCU USER UART2 RX)|
    +-----------------+-----------------------------+
..
    **3.5 ESP32 DEBUG connector (J5)**
..
    10-pin connector J5 used for connecting J-Link to the ESP32. It has standard pin-out.
..
    +-------------------+-------------------------+
    | **Pin**           |   Main Function         |
    |                   |                         |
    +-------------------+-------------------------+
    | 1                 | VDD 3V3                 |
    +-------------------+-------------------------+
    | 3, 5, 9           | GND                     |
    +-------------------+-------------------------+
    | 2                 | ESP32_TMS               |
    +-------------------+-------------------------+
    | 4                 | ESP32_TCK               |
    +-------------------+-------------------------+
    | 6                 | ESP32_TDO               |
    +-------------------+-------------------------+
    | 8                 | ESP32_TDI               |
    +-------------------+-------------------------+
    | 10                | ESP32_RESET             |
    +-------------------+-------------------------+

..   
    The OpenRTK evaluation board has a bluetooth module. You can use our custom APP to set some parameters and work as a NTRIP client. 
    The detailed content are described in the table on the “QUICK START » How to use OpenRTK? » Mobile” page accessible from the Contents bar on the left.
..
**OpenRTK EVK Important Notice**

::

     This evaluation kit is intended for use for FURTHER ENGINEERING, DEVELOPMENT, 
     DEMONSTRATION, OR EVALUATION PURPOSES ONLY. It is not a finished product and may not 
     (yet) comply with some or any technical or legal requirements that are applicable to finished 
     products, including, without limitation, directives regarding electromagnetic compatibility, 
     recycling (WEEE), FCC, CE or UL (except as may be otherwise noted on the board/kit). Aceinna 
     supplied this board/kit "AS IS," without any warranties, with all faults, at the buyer's and further 
     users' sole risk. The user assumes all responsibility and liability for proper and safe handling of the 
     goods. Further, the user indemnifies Aceinna from all claims arising from the handling or use of 
     the goods. Due to the open construction of the product, it is the user's responsibility to take any 
     and all appropriate precautions with regard to electrostatic discharge and any other technical or 
     legal concerns. EXCEPT TO THE EXTENT OF THE INDEMNITY SET FORTH ABOVE, NEITHER USER 
     NOR ACEINNA SHALL BE LIABLE TO EACH OTHER FOR ANY INDIRECT, SPECIAL, INCIDENTAL, OR 
     CONSEQUENTIAL DAMAGES. No license is granted under any patent right or other intellectual 
     property right of Aceinna covering or relating to any machine, process, or combination in which 
     such Aceinna products or services might be or are used.
