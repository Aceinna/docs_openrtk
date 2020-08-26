Quick Start
==========================

.. contents:: Contents
    :local:

OpenRTK330LI EVK Introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The OpenRTK330LI Evalution Kit (EVK) is designed to evaluate the OpenRTK330LI module with the  online Aceinna Navigation Studio (ANS) and related software stack. A full set of OpenRTK330 EVK is shown below after you unpack the product box. 

.. figure:: media/EvalKit.png
    :width: 7.0in
    :height: 5.0in

where

  * 1: ST-Link debugger
  * 2: Multi-frequency GNSS antenna
  * 3: Micro-USB cable
  * 4: OpenRTK330 Evaluation Board (EVB) with metal flat mounting board
  * 5: 12-V DC adapter with 5.5 x 2.1 mm power jack

The picture below shows the detailed overview of OpenRTK330 EVB

  .. figure:: media/EvalBoard.png
      :width: 6.0in
      :height: 6.0in

where some of the parts are listed here

  * 1: OpenRTK330 GNSS/IMU integrated module
  * 2: GNSS antenna SMA interface
  * 3: Espressif ESP32 bluetooth module
  * 4: SWD/JTAG connector, 20-pin
  * 7: Boot mode swtich, with two positions (A, B) to select from
  * 8: RJ45 jack for Ethernet connection
  * 9: Micro-USB port
  * 10: 9-pin CAN interface
  * 12: EVB working status LEDs, yellow, red, and green LED from left to right

.. The ESP32 bluetooth module on the OpenRTK330 EVB has been programmed and configured to provide bluetooth wireless connectivity, and user do not have to get hands on it.

.. The OpenRTK330 EVK is all set as a high precision GNSS/INS positioning platform before shipping out, you could skip the following firmware installation/update process and directly go to learn `How to Use OpenRTK330 EVK <https://openrtk.readthedocs.io/en/latest/useOpenRTK.html>`_. Otherwise, if you want to update the module with the latest firmware, follow the instructions below carefully.


OpenRTK Python driver
~~~~~~~~~~~~~~~~~~~~~~~


Quick Setup
~~~~~~~~~~~~~

1. Connect the EVB with a PC using a Micro-USB cable, and the **YELLOW** LED (#12 on the EVB figure above) flashes. The EVB is powered on, and four UART com ports are established on the PC, 

2. Connect a GNSS multi-frequency antenna to the SMA interface (#2 on the EVB figure), the **GREEN** LED (#12 on the EVB figure above) flashes if the incoming GNSS signal is valid

3. Refer to next section `How to Use OpenRTK330 <https://openrtk.readthedocs.io/en/latest/useOpenRTK.html>`_ to utilize the provided user App for RTK/INS operation, data logging and etc.



