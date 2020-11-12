The OpenRTK330LI Module
==========================

The Aceinna OpenRTK330 module integrates a ST Teseo V automotive grade
multi-constellation, multi-frequency Global Navigation Satellite System
(**GNSS**) chipset (supports GPS, GALILEO, GLONASS, Beidou, QZSS), a
triple-redundant 6-axis (3-axis accelerometer and 3-axis gyro) **MEMS**
Inertial Measurement Unit (**IMU**), and a ST M4 MCU as the processor.
OpenRTK330 module is targeted for commecial applicaiton for the mass
market that requires a reliable, high-precision and yet cost effective
**GNSS/INS** integrated positioning solution.

Features with:

  * 100 Hz GNSS/INS integrated position, velocity and attitude solution
  * Integrated tripple redundant 6-axis IMU sensors
  * Integrated multi-frequency GNSS chipset with the following two frequency plans
 
    +-----------+-----------------------+-----------------------+
    | GNSS      | L1/L2 plan            | L1/L5 plan            |
    +-----------+-----------------------+-----------------------+
    | GPS       | L1 C/A + L2C          | L1 C/A + L5           |
    +-----------+-----------------------+-----------------------+
    | GLONASS   | G1                    | G1                    |
    +-----------+-----------------------+-----------------------+
    | BeiDou    | B1I + B2I             | B1I + B2A             |
    +-----------+-----------------------+-----------------------+
    | Galileo   | E1 + E5b              | E1 + E5b              |
    +-----------+-----------------------+-----------------------+
    | QZSS      | L1 C/A + L2C          | L1 C/A + L5           |
    +-----------+-----------------------+-----------------------+

  * RTK algorithms on-board for up to centimetre accuracy
  * UART / SPI / CAN / Ethernet Interfaces


.. toctree::
    :maxdepth: 2
    :hidden:

    Module-OpenRTK330LI/technical_specs
    Module-OpenRTK330LI/communication_ports