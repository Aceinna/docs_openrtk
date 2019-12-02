OpenRTK330LI
============

1.0 Introduction
----------------

The Aceinna OpenRTK330LI module integrates a commercial
multi-constellation, multi-frequency Global Navigation Satellite System
(**GNSS**) chipset (supports GPS, GALILEO, GLONASS and Beidou), a
triple-redundant 6-axis (3-axis accelerometer and 3-axis gyro) **MEMS**
Inertial Measurement Unit (**IMU**), and a ST M4 MCU as the processor.
OpenRTK330LI module is targeted for commecial applicaiton for the mass
market that requires a reliable, high-precision and yet cost effective
**GNSS/INS** integrated positioning solution.

-  100 Hz GNSS/INS integrated position, velocity and attitude solution
-  Integrated tripple redundant IMU (OpenIMU330)
-  Integrated multi-frequency GNSS chipset
-  180MHz STM32 M4 CPU with FPU
-  UART / SPI / CAN / Ethernet Interfaces
-  Sensors Update Rate to 800Hz
-  GPS (L1/L2 or L1/L5), GLONASS (L1/L2), GALILEO (E1/E5), Beidou
   (B1I/B2I),QZSS (L1), and SBAS
-  RTK/PPP algorithms on-board for up to centimetre accuracy

2.0 Detailed specifications:
----------------------------

UART PORTS
~~~~~~~~~~

-  Debug UART
-  Default Baud Rate: 460.8kb/s
-  Default Message: gga and rtk algorithm time
-  USER UART
-  Default Baud Rate: 460.8kb/s
-  Default User Message: e1 packet, output rate 100Hz, including scaled
   Lat/Long, Roll, Pitch, Heading, and Angular Rate
-  Default Map Message: pos packet('pS'), snr packet('sR') and skyview
   packet('sK'), output rate 10Hz
-  Input User Messages: 'pG' - ping, 'gV' - version, 'gA' - get all
   params, 'gP' - get a param, 'sC' - save current configuration
   permanently, 'uP'- update param. Params include Baud Rate, Packet
   Type, Packet Rate, Accel LPF, Rate LPF, and Orientation
-  GNSS UART
-  Update GNSS chipset firmware
-  Default Baud Rate: 460.8kb/s
-  Default Message: rtcm3, 10Hz
-  ESP32 UART
-  Update bluetooth firmware
-  Default Baud Rate: 460.8kb/s
-  Default Message: Bluetooth Debug

SPI PORT
~~~~~~~~

-  TBD

CAN PORT
~~~~~~~~

-  TBD

Bluetooth and Ethernet mode
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The OpenRTK300LI can be configured in a number of ways for communication
with NTRIP server. There are up to bluetooth mode and ethernet mode.

Bluetooth mode
^^^^^^^^^^^^^^

-  OpenRTK330LI acts as NTRIP client connects with NTRIP server via
   Android smartphone (with 4G) Bluetooth connectivity (with Aceinna
   RTKTool App installed) to fetch GNSS RTK/PPP correction data stream
-  Default bluetooth device name "OpenRTK\_0001" shows on Android
   smartphone, which can be changed through installed Aceinna RTKTool
   App
-  Configure NTRIP server settings on Anroid smartphone in the provided
   Aceinna RTKTool App

Ethernet mode
^^^^^^^^^^^^^

-  Plug in a RJ45 cable from a local network to OpenRTK330LI Ethernet
   port
-  OpenRTK330LI acts as NTRIP client connects with NTRIP server via host
   (e.g. Desktop) to fetch GNSS RTK/PPP correction data stream
-  DHCP IP address is used as default, if no success, manually setup a
   static IP: ip = 192.168.1.110, netmask = 255.255.255.0, gateway =
   192.168.1.1
-  The embedded webserver address is "http://opentrk"

