Quick Start
===================

.. contents:: Contents
    :local:

**Note: if the figures are blur, click on the figure to see the clearer version**

OpenRTK330LI EVK Introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The OpenRTK330LI Evalution Kit (EVK) is designed to evaluate the OpenRTK330LI module with the  online Aceinna Navigation Studio (ANS) and related software stack. A full set of OpenRTK330 EVK is shown below after you unpack the product box. 

.. figure:: media/EvalKit.png
    :width: 7.0in
    :height: 5.0in

where

  * 1: ST-Link debugger
  * 2: Multi-Constellation and Multi-frequency GNSS antenna, supports

    - GPS L1/L2/L5
    - GLONASS L1/L2
    - GALILEO E1/E5/E6
    - BEIDOU B1/B2

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
  * 7: Boot mode swtich

      - Position A: booting from bootloader
      - Position B: normal working mode
  * 8: RJ45 jack for Ethernet connection
  * 9: Micro-USB port
  * 10: 9-pin CAN interface
      
      - Pin-7: CAN_H signal
      - Pin-2: CAN_L signal
  * 12: EVB working status LEDs, yellow, red, and green LED from left to right

.. The ESP32 bluetooth module on the OpenRTK330 EVB has been programmed and configured to provide bluetooth wireless connectivity, and user do not have to get hands on it.

.. The OpenRTK330 EVK is all set as a high precision GNSS/INS positioning platform before shipping out, you could skip the following firmware installation/update process and directly go to learn `How to Use OpenRTK330 EVK <https://openrtk.readthedocs.io/en/latest/useOpenRTK.html>`_. Otherwise, if you want to update the module with the latest firmware, follow the instructions below carefully.


.. The OpenRTK Python driver
.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. The OpenRTK Python driver is an open source Python 


Quick Setup and Usage
~~~~~~~~~~~~~~~~~~~~~~~

Prerequisites
^^^^^^^^^^^^^^^^^^^

**Hardware**

  * OpenRTK330LI EVK 
  * Ethernet cable (must have, not included in the EVK)
  * Ethernet router/network switch (optional, not included in the EVK)

**Software**

  * The online Aceinna Navigation Studio (`ANS <https://developers.aceinna.com/devices/rtk>`_) deverloper website, manily for

    * OpenRTK devices management and technical forum and support
    * Web-based Graphical User Interface (GUI)
    * App center for online firmware upgrade

  * The OpenRTK Python driver: Python based program runs on a PC, click `here <https://github.com/Aceinna/python-openimu/releases/>`_ to download the latest version of executables

    * Send/Receive data from ANS to enable Web GUI and online firmware upgrade for OpenRTK330LI device
    * Log and parse OpenRTK330LI output data, positioning solution and other debug information to binary and text files

Usage Steps
^^^^^^^^^^^^^^^^^

1. **Power and data link**: connect the EVB with a PC using a Micro-USB cable, and the **YELLOW** LED (#12 on the EVB figure above) flashes. The EVB is powered on, and four serial com ports are established on the PC. 

2. **Antenna**: connect a GNSS multi-frequency antenna to the SMA interface (#2 on the EVB figure), the **GREEN** LED (#12 on the EVB figure above) flashes if the incoming GNSS signal is valid

3. **Network**: Use an Ethernet calbe to connect the EVB with a network router or switch, and then connect a PC to the same router/switch using an Ethernet cable. The OpenRTK330LI EVB gets internet access and assigned an IP address in the local network via DHCP.

4. **GNSS RTK and INS Configuration**: open a browser (Google Chrome is recommended), visit http://openrtk, 

  * You will firstly see the following device running status page

      .. image:: media/Web_RunningStatus.png
                :align: center
                :scale: 40%

  * On the left side menu bar, click "Work Configuration" tab to choose the following working mode of the device and configure it accordingly: 
  
    - Rover: works as a nomarl GNSS positioning unit that is also referring to "NTRIP client" receiving GNSS data correction
    - Base: works as a GNSS reference station with known position and sending GNSS data to "NTRIP server" to be used as GNSS data correction

    Please refer to the `"How-to-use" <https://openrtk.readthedocs.io/en/latest/useOpenRTK.html>`_ chapter for the detailed configurations.

        .. image:: media/Web_WorkModeNtrip.png
                :align: center
                :scale: 40%

  .. * On the left side menu bar, click "INS Configuration" tab to enter necessary parameters for INS algoritm to work

  ..       .. image:: media/Web_INSConfig.png
  ..               :align: center
  ..               :scale: 40%


  * On the left side menu bar, click "User Configuration" tab to select the user output data and rate among the options provided, including NMEA0183 messages and Aceinna format binaries

        .. image:: media/Web_UserConfig.png
              :align: center
              :scale: 40%

  * On the left side menu bar, click "Device Info" tab to have the detailed device information displayed, including firmware version, product number and serial number etc..

        .. image:: media/Web_DeviceInfo.png
              :align: center
              :scale: 40%       

.. 5. **Firmware Version Check**: unzip the previously downloaded Python driver executables (v2.1.6 and later), and run the driver executable on a command line, for example:

..   .. code-block:: python

..           cd c:\pythondriver-win
..           .\ans-devices.exe

..   Check the console output like below, make sure the RTK_INS App version is v2.0.0 and later. Otherwise, follow `these steps <https://openrtk.readthedocs.io/en/latest/firmware_upgrade.html>`_ to upgrade the device's firmware first

..          .. image:: media/python_driver_connects.png
..               :align: center
..               :scale: 50%

6. **Live Web GUI**: when the Python driver is running and connects with the device correctly, 

  * Go to online ANS (deverlopers.aceinna.com), on the left side menu bar, click "Devices"->"OpenRTK", then we will have the "OpenRTK Monitor" webpage as shown below, and the center "Play" button is highlighted indicating correct device connection with the Web GUI, 

        .. image:: media/web_gui_connect.png
              :align: center
              :scale: 50%
  
  * Click "Play", you will have a live web GUI showing positioning information, map presentation and other satellites information

      .. image:: media/web_gui_play.png
              :align: center
              :scale: 50%


7. **Data Logging and Parsing**: every time the Python driver is running, the Python driver is logging all raw data, positioning solution and debug information output from the device, and create a subfolder contains all the binary file logs in the same folder as the Python driver executable

      .. image:: media/python_driver_logging.png
              :align: center
              :scale: 50%

  Navigate to the "openrtk_data_parse" subfolder, run the parser executable as below

    .. code-block:: python

          cd c:\pythondriver-win\openrtk_data_parse
          .\openrtk_parse.exe -p ..\data\openrtk_log_20200828_153600

  Then, the logged binary files are decoded into text files for post-processing and analysis.


Note
~~~~~~~

This section presents a brief introduction and quick start on using OpenRTK330LI EVK for RTK and INS positioning. Please refer to the remaining sections of this tutorial chapter to explore more on OpenRTK330LI's features and its usage.





  