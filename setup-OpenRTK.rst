OpenRTK330 Evaluation Kit Setup
=================================
.. contents:: Contents
    :local:

Please refer to the figure below for setting up the OpenRTK330 Evaluation Kit (EVK) with the following steps

.. figure:: media/EvalKit.png
    :width: 6.0in
    :height: 6.0in

EVK setup and firmware download
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*  Unpack the OpenRTK330 EVK, you will find the following items

    * OpenRTK330 Evaluation Board (EVB)
    * ST-Link Debugger
    * Micro USB cable
    * Mounting plate
* Connect the micro USB port on the OpenRTK330 EVB to a PC via a USB cable, four serial ports will be established on your PC, and the EVB is also powered up by this USB connection. In addition, the EVB can also be powered up directly by a 9-12v DC adapter/generator. In this case, the USB connection is just a data link. The red LED is always on if powered up.
* Connect the ST-Link debugger to a PC's USB port.
* Connect the other end of the ST-Link debugger to the SWD/JGAG pin connector on OpenRTK330 EVB.
* Connect the SMA female connector with a satellite antenna (OpenRTK330 can power on the antenna if passive, otherwise use a DC blocker)
* Download the OpenRTK330 firmware from Aceinna's developer website https://developers.aceinna.com/code/apps, as shown by
.. figure:: media/download_openrtk330_firmware.png
    :width: 7.0in
    :height: 4.0in

* Download and install the STM32 ST-LINK Utility software from https://www.st.com/en/development-tools/stsw-link004.html
* Open the STM32 ST-LINK Utility software, and click the red circled "1" to establish a connection with the OpenRTK EVB
.. figure:: media/st-link_utility_flash_firmware1.png
    :width: 7.0in
    :height: 4.0in

* Click the red circled "2" to open the firmware flashing dialog, change the start address to "0x8010000", and browse to load the downloaded OpenRTK330 firmware bin file, then click "Start"
.. figure:: media/st-link_utility_flash_firmware2.png
    :width: 5.0in
    :height: 3.0in

At this point, the OpenRTK330 firmware is loaded and ready for GNSS RTK positioning that also requires internet connection to a NTRIP server for GNSS data correction. By default, the ESP32 module on the OpenRTK330 EVB has been programmed and configured to have Bluetooth wireless connectivity, and then connects with Aceinna's OpenRTK Android App for internet connectivity (see next section). Alternatively, the following step can be performed to get internet connectivity

*   (optional) Connect the EVB (RJ45 connector) with a network router/gateway with an Ethernet cable, the usage of this connection will also be addressed in next section

 
Online firmware upgrade
~~~~~~~~~~~~~~~~~~~~~~~
There are two steps to upgrade the OpenRTK330 firmware online. First, go to Aceinna's github page and clone the repo "python-openimu", click here (https://github.com/Aceinna/python-openimu), and checkout the "ans-devices" branch. Run the following commands:

.. code-block:: python

    cd .\python-openimu
    python main.py

The python driver automatically scans available USB-serial ports and finds the right com port.    

Secondly, log in to Aceinna's developer website (https://developers.aceinna.com) and navigate to Code->Apps on the left side menu, click the OpenRTK330 App as shown in the following figure. If the correct com port is found by the python driver, the "upgrade" button is highlighted. Click the "upgrade" button and wait for the firmware upgrade completes.

.. figure:: media/app_upgrade_openRTK.png
    :width: 7.0in
    :height: 4.0in
