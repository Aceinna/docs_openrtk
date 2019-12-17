OpenRTK330 Module
=================

The Aceinna OpenRTK330 module integrates a ST Teseo V automotive grade
multi-constellation, multi-frequency Global Navigation Satellite System
(**GNSS**) chipset (supports GPS, GALILEO, GLONASS, Beidou, QZSS and NAVIC), a
triple-redundant 6-axis (3-axis accelerometer and 3-axis gyro) **MEMS**
Inertial Measurement Unit (**IMU**), and a ST M4 MCU as the processor.
OpenRTK330 module is targeted for commecial applicaiton for the mass
market that requires a reliable, high-precision and yet cost effective
**GNSS/INS** integrated positioning solution.

-  100 Hz GNSS/INS integrated position, velocity and attitude solution
-  Integrated tripple redundant 6 axis IMU sensors
-  Integrated multi-frequency GNSS chipset
-  180MHz STM32 M4 CPU with FPU
-  UART / SPI / CAN / Ethernet Interfaces
-  Sensors Update Rate to xxxHz
-  GPS (L1/L2 or L1/L5), GLONASS (L1/L2), GALILEO (E1/E5), Beidou
   (B1I/B2I),QZSS (L1), and SBAS
-  RTK/PPP algorithms on-board for up to centimetre accuracy

Technical characteristics
-------------------------

 +-------------------------------------------------------------------+
 | **Accuracy** [#f1]_                                               |
 +-------------------------------------------------------------------+
 | *Horizontal Position Accuracy (RMS)*                              |
 +----------------------------------------+--------------------------+
 | SPS                                    | 1.2 m CEP                |
 +----------------------------------------+--------------------------+
 | SBAS                                   | 0.6 m                    |
 +----------------------------------------+--------------------------+
 | DGPS                                   | 0.4 m                    |
 +----------------------------------------+--------------------------+
 | RTK [#f2]_                             | 0.02 m                   |
 +----------------------------------------+--------------------------+
 | 10s GNSS Outage                        | 0.3 m                    |
 +----------------------------------------+--------------------------+
 | *Vertical Position Accuracy (RMS)*                                |
 +----------------------------------------+--------------------------+
 | SPS                                    | 1.8 m CEP                |
 +----------------------------------------+--------------------------+
 | RTK                                    | 0.03 m                   |
 +----------------------------------------+--------------------------+
 | 10s GNSS Outage                        | 0.4 m                    |
 +-------------------------------------------------------------------+
 | *Velocity Accuracy (RMS)*                                         |
 +----------------------------------------+--------------------------+
 | Horizontal                             | 0.01 m/s                 |
 +----------------------------------------+--------------------------+
 | Vertical                               | 0.02 m/s                 |
 +----------------------------------------+--------------------------+
 | Heading Accuracy (RMS) [#f3]_          | 0.5°                     |
 +----------------------------------------+--------------------------+
 | Attitude Accuracy (Roll/Pitch, RMS)    | 0.1°                     |
 +----------------------------------------+--------------------------+
 | *Operating Limits*                                                |
 +----------------------------------------+--------------------------+
 | Velocity                               | 515 m/s                  |
 +----------------------------------------+--------------------------+
 | Acceleration                           | 8 g                      |
 +----------------------------------------+--------------------------+
 | Angular Rate                           | 400 °/s                  |
 +----------------------------------------+--------------------------+
 | Temperature Calibration Range          | -40 °C to +85 °C         |
 +----------------------------------------+--------------------------+
 | **Timing**                                                        |
 +-------------------------------------------------------------------+
 | *Time to First Fix* [#f4]_                                        |
 +----------------------------------------+--------------------------+
 | Cold Start [#f5]_                      | <60 s                    |
 +----------------------------------------+--------------------------+
 | Warm Start [#f6]_                      | <45 s                    |
 +----------------------------------------+--------------------------+
 | Hot Start                              | <11 s                    |
 +----------------------------------------+--------------------------+
 | Signal Re-acquisition                  | <2 s                     |
 +----------------------------------------+--------------------------+
 | RTK Initialization Time                | <1 min                   |
 +----------------------------------------+--------------------------+
 | Update Rate                            | 10 Hz                    |
 +----------------------------------------+--------------------------+
 | Output Data Rate                       | 100 Hz (200 Hz MAX)      |
 +----------------------------------------+--------------------------+
 | **Sensitivity**                                                   |
 +----------------------------------------+--------------------------+
 | Tracking                               | -160 dBm                 |
 +----------------------------------------+--------------------------+
 | Cold Start                             | -140 dBm                 |
 +----------------------------------------+--------------------------+
 | **Environment**                                                   |
 +----------------------------------------+--------------------------+
 | Operating Temperature (°C)             | -40 to +85               |
 +----------------------------------------+--------------------------+
 | Non-Operating Temperature (°C)         | -55 to +105              |
 +----------------------------------------+--------------------------+
 | Vibration                              | IEC 60068-2-6 (5g)       |
 +----------------------------------------+--------------------------+
 | Shock survival                         | MIL-STD-810G (40g)       |
 +----------------------------------------+--------------------------+
 | **Electrical**                                                    |
 +----------------------------------------+--------------------------+
 | Input Voltage (VDC)                    | 2.7 to 5.5 V             |
 +----------------------------------------+--------------------------+
 | Power Consumption (W)                  | 1.0 (Typical)            |
 +----------------------------------------+--------------------------+
 | Digital Interface                      | Ethernet, CAN, UART, SPI |
 +----------------------------------------+--------------------------+
 | **Physical**                                                      |
 +----------------------------------------+--------------------------+
 | Package Type                           | 50-pin LGA               |
 +----------------------------------------+--------------------------+
 | Size (mm)                              | 30 x 30 x                |
 +----------------------------------------+--------------------------+
 | Weight (gm)                            | 5                        |
 +----------------------------------------+--------------------------+
  
 .. rubric:: Notes
 
 .. [#f1] Typ values, subject to ionospheric/tropospheric conditions, satellite geometry, 
          baseline length, multipath and interference effects.

 .. [#f2] Add 1ppm of baseline length.

 .. [#f3] After dynamic motion initialization. 

 .. [#f4] Typical values.

 .. [#f5] No previous satellite or position information.

 .. [#f6] Using ephemeris and last known position.


Communication ports definitions
-----------------------
There are six serial communicatoins ports available on the OpenRTK330 module, including four configurable UART ports, one SPI port and one CAN port.

UART PORTS
~~~~~~~~~~
The default configuration of the four UART ports is listed as follows

-  USER port

    -  default baud rate: 460800 b/s
    -  default navigation messages: GNSS PV (positioin and velocity) packet ('pS'), satellite SNR (Signal-to-Noise Ratio), elevation and azimuth packet ('sK')
-  ST_UART1 port

    -  default baud rate: 460800 b/s
-  DEBUG port

    -  default baud rate: 460800 b/s
    -  default message: raw IMU data packet ('e1')
-  ST_UART_PROG port

    -  default baud rate: 460800 b/s
    -  default message: RTCMv3 GNSS data stream (10 Hz)

SPI PORT
~~~~~~~~

-  TBD

CAN PORT
~~~~~~~~

-  TBD

OpenRTK330 Pin Definition Rev
-----------------------------

 .. image:: media/OpenRTK330LI_pin_n.png

 +---------+-------------------+----------+-----------------------------------------------------------------+
 | **No.** | **Name**          | **Type** | **Description**                                                 |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      1  |  GND              |      P   | Ground                                                          |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      2  |  GND              |      P   | Ground                                                          |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      3  |  GND              |      P   | Ground                                                          |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      4  |  GND              |      P   | Ground                                                          |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      5  |  VBAT             |      P   | Reserved                                                        |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      6  |  LED2             |      O   | Status2 LED                                                     |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      7  |  LED1             |      O   | Status1 LED                                                     |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      8  |  ETH_RESET        |      O   | Reset signal of ETH RMII interface                              |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      9  |  RMII_TXD0        |      O   | Transmit data0 of ETH RMII interface                            |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      10 |  RMII_TXD1        |      O   | Transmit data1 of ETH RMII interface                            |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      11 |  RMII_TX_EN       |      O   | Transmit enable of ETH RMII interface                           |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      12 |  VDD_CORE         |      P   | Reserved                                                        |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      13 |  VIN              |      P   | Typical DC3.3V, input voltage DC3.0V~3.6V                       |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      14 |  RMII_RXD1        |      I   | Receive data1 of ETH RMII interface                             |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      15 |  ETH_MDC          |      O   | Management interface (MII) clock output                         |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      16 |  RMII_RXD0        |      I   | Receive data0 of ETH RMII interface                             |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      17 |  RMII_REF_CLK     |      I   | Clock signal of ETH RMII Interface                              |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      18 |  ETH_MDIO         |     I/O  | Management interface (MII) data I/O                             |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      19 |  RMII_CRS_DV      |      O   | Carrier sense/receive data valid output of ETH RMII interface   |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      20 |  GND              |      P   | Ground                                                          |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      21 |  GNSS_1PPS        |      I   | 1PPS signal from external GNSS module                           |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      22 |  GNSS_RTK_STAT    |      I   | RTK status signal from external GNSS module                     |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      23 |  GNSS_RSTn        |      O   | Reset signal to external GNSS module                            |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      24 |  GNSS_TX          |      I   | Receive data from external GNSS module                          |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      25 |  GNSS_RX          |      O   | Transmit data to external GNSS module                           |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      26 |  DEBUG_NRST       |      I   | Reset signal of MCU debug interface                             |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      27 |  WIFI/BT_RESET    |      O   | Rest signal for external WIFI/BT module                         |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      28 |  WIFI/BT_BOOT_CTL |     O    | Boot mode select signal for external WIFI/BT module             |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      29 |  USER_MOSI        |     I    | SPI interface.  Receive data from master                        |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      30 |  USER_SCK         |     I    | SPI interface. Clock signal from master                         |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      31 |  USER_NSS         |     I    | SPI interface. Chip selected signal from master                 |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      32 |  USER_MISO        |     O    | SPI interface. Transmit data to master                          |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      33 |  LED3             |     O    | Status3 LED                                                     |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      34 |  ST_BOOT_MODE     |     I    | Boot mode control signal for internal ST GNSS chip              |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      35 |  WIFI/BT_UART2_RX |     I    | Receive data from external WiFi/BTmodule                        |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      36 |  WIFI/BT_UART2_TX |     O    | Transmit data to external WiFi/BT module                        |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      37 |  CAN_AB           |     O    | CAN bus transceiver loopback mode control                       |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      38 |  CAN_120R_CTL     |     O    | CAN termination resistor control (ON/OFF)                       |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      39 |  USER-DRDY        |     O    | Data ready signal                                               |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      40 |  GND              |     P    | Ground                                                          |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      41 |  LTE1_TX          |     O    | Transmit data to external LTE module 1                          |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      42 |  LTE1_RX          |     I    | Receive data from external LTE module 1                         |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      43 |  LTE1_PWR         |     O    | Power control signal for external LTE module 1                  |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      44 |  LTE1_RSTn        |     O    | Reset signal of external LTE module 1                           |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      45 |  LTE2_RSTn        |     O    | Reset signal of external LTE module 2                           |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      46 |  GND              |     P    | Ground                                                          |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      47 |  LTE2_RX          |     I    | Receive data from external LTE module 2                         |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      48 |  LTE2_TX          |     O    | Transmit data to external LTE module 2                          |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      49 |  ST_UART_PROG_TX  |     O    | Receive data from internal ST GNSS UART2 (GNSS program burning) |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      50 |  ST_UART_PROG_RX  |     I    | Transmit data to internal ST GNSS UART2 (GNSS program burning)  |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      51 |  DEBUG-TX         |     O    | Transmit data. Debug serial port                                |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      52 |  DEBUG-RX         |     I    | Receive data. Debug serial port                                 |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      53 |  CAN_RX           |     I    | Receive data from CAN bus                                       |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      54 |  CAN_TX           |     O    | Transmit data to CAN bus                                        |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      55 |  USER_UART1_RX    |     I    | Receive data. User serial channel 1                             |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      56 |  USER_UART1_TX    |     O    | Transmit data. User serial channel 1                            |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      57 |  SWDIO            |     I/O  | Data IO of SWD debug interface                                  |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      58 |  SWCLK            |     I    | Clock signal of SWD debug interface                             |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      59 |  ST_UART1_TX      |     O    | Transmit data from internal ST GNSS UART1 port (debug data)     |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      60 |  ST_UART1_RX      |     I    | Receive data to internal ST GNSS UART1 port (debug data)        |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      61 |  1PPS             |     O    | 1PPS signal                                                     |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      62 |  LTE2_PWR         |     O    | Power control signal for external LTE module 2                  |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      63 |  LNA_EN           |     O    | Control signal of external LNA power                            |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      64 |  ANT_EN           |     O    | ANtenna enable, reserved                                        |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      65 |  ANT_SENSE        |     I    | Antenna sensing detection, reserved                             |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      66 |  AGND             |     P    | Internal GNSS RF path ground                                    |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      67 |  ANT_IN           |     I    | GNSS antenna signal input                                       |
 +---------+-------------------+----------+-----------------------------------------------------------------+
 |      68 |  AGND             |     P    | Internal GNSS RF path ground                                    |
 +---------+-------------------+----------+-----------------------------------------------------------------+




.. Bluetooth and Ethernet mode
.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. The OpenRTK300LI can be configured in a number of ways for communication
.. with NTRIP server. There are up to bluetooth mode and ethernet mode.

.. Bluetooth mode
.. ^^^^^^^^^^^^^^

.. -  OpenRTK330 acts as NTRIP client connects with NTRIP server via
..    Android smartphone (with 4G) Bluetooth connectivity (with Aceinna
..    RTKTool App installed) to fetch GNSS RTK/PPP correction data stream
.. -  Default bluetooth device name "OpenRTK\_0001" shows on Android
..    smartphone, which can be changed through installed Aceinna RTKTool
..    App
.. -  Configure NTRIP server settings on Anroid smartphone in the provided
..    Aceinna RTKTool App

.. Ethernet mode
.. ^^^^^^^^^^^^^

.. -  Plug in a RJ45 cable from a local network to OpenRTK330 Ethernet
..    port
.. -  OpenRTK330 acts as NTRIP client connects with NTRIP server via host
..    (e.g. Desktop) to fetch GNSS RTK/PPP correction data stream
.. -  DHCP IP address is used as default, if no success, manually setup a
..    static IP: ip = 192.168.1.110, netmask = 255.255.255.0, gateway =
..    192.168.1.1
.. -  The embedded webserver address is "http://opentrk"

