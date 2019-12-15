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
-  ESP32 port

    -  default baud rate: 460800 b/s
-  DEBUG port

    -  default baud rate: 460800 b/s
    -  default message: raw IMU data packet ('e1')
-  GNSS RTCM data port

    -  default baud rate: 460800 b/s
    -  default message: RTCMv3 GNSS data stream (10 Hz)

SPI PORT
~~~~~~~~

-  TBD

CAN PORT
~~~~~~~~

-  TBD

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

