The OpenRTK330LI EVK
=====================

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

    .. image:: ./media/EvalKit.png

where

  * 1: ST-Link debugger
  * 2: Multi-Constellation Multi-frequency GNSS antenna
  * 3: Micro-USB cable
  * 4: OpenRTK330 Evaluation Board (EVB) with metal flat mounting board
  * 5: 12-V DC adapter with 5.5 x 2.1 mm power jack

**2. OpenRTK330 EVB**
~~~~~~~~~~~~~~~~~~~~~~~

An OpenRTK330 Evaluation board is shown below in detail

 .. figure:: ./media/EvalBoard.png
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
  
     - Not compatible with power over ethernet devices
     
  - 9. Micro-USB port
  - 10. CAN interface
  - 11. Power jack for 12-v adapter
  - 12. EVB working status LEDs from left to right 

    - Yellow: ST GNSS chipset is powered on and working properly
    - Red: valid GNSS base station data receiving
    - Green: valid GNSS signal receiving


.. The following pages cover:

.. *   Detailed Overview
..
    *   Eval Board Mechanical Drawing
    *   Eval Board Schematic


.. toctree::
    :maxdepth: 2
    :hidden:

    EVK-OpenRTK330LI/mechanical
    EVK-OpenRTK330LI/schematic

.. EVK-OpenRTK330LI/overview

   
   
