OpenRTK330 EVK Setup
=================================

.. contents:: Contents
    :local:

The OpenRTK330 Evalution Kit (EVK) is designed to evaluate the performance of the GNSS/INS positioning with OpenRTK330 module and the OpenRTK software stack. The major components of the OpenRTK330 EVK are shown by the figure below, followed by steps to quickly setup the EVK and install the firmware on the OpenRTK330 module. 

.. figure:: media/EvalKit.png
    :width: 6.0in
    :height: 6.0in

EVK setup and firmware installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Unpack the OpenRTK330 EVK, you will find the following items for the EVK quick setup and firmware installation

    * OpenRTK330 Evaluation Board (EVB)
    * ST-Link Debugger
    * Micro USB cable
    * Mounting plate

By factory settings, the ESP32 module on the OpenRTK330 EVB has been programmed and configured to provide Bluetooth wireless connectivity. The Ethernet and CAN drivers are also set.

1. Mount the EVB on the mounting plate for easy operation.

2. Connect the micro USB port on the OpenRTK330 EVB to a PC via a USB cable, four serial ports will be established on your PC, and the EVB is also powered up by this USB connection. Alternatively, the EVB can be powered up directly by a 9-12v DC adapter/generator. In this case, the USB connection is just a data link. The red LED by the micro-USB is always on if powered up.

3. Install the OpenRTK330 module firmware from App center of `Aceinna's developer website <https://developers.aceinna.com/code/apps>`_, as shown by

    .. figure:: media/download_openrtk330_firmware.png
        :width: 6.5in
        :height: 3.0in

    Click "RTK_INS" App, there are **two options** to install the OpenRTK330 firmware online:

        .. figure:: media/app_upgrade.png
            :width: 6.5in
            :height: 4.0in

    1. (Recommended) The **online upgrade** way  
    
        - Download and run the python driver executables from a command line
          
            - `Windows 10 <https://github.com/Aceinna/python-openimu/files/4211970/ans-devices-win.zip>`_

            - `Mac OS <https://github.com/Aceinna/python-openimu/files/4211966/ans-devices-mac.zip>`_

        - If you prefer building from source, go to Aceinna's github page and clone the repo `python-openimu <https://github.com/Aceinna/python-openimu>`_, and checkout the "ans-devices" branch. Run the OpenRTK Python driver with the following commands:

            .. code-block:: python

                cd .\python-openimu
                pip install -r requirements.txt
                python main.py

        The python driver automatically scans available USB-serial ports and finds the right com port. If the correct com port is found, the "UPGRADE" button circled by cyan rectrangle will be highlighted. Then click the "UPGRADE" button to start the firmware upgrade process and wait it completes.  

    2. (Option 2) **Download** the firmware bin file and **flash** it into OpenRTK330 module. In order to fullfill this, first install the STM32 ST-LINK Utility software from https://www.st.com/en/development-tools/stsw-link004.htm on your PC. Then open the STM32 ST-LINK Utility software and connect the OpenRTK330 EVB with PC using the ST-LINK debugger,

        1. Click the red circled "1" to establish a connection with the OpenRTK EVB

            .. figure:: media/st-link_utility_flash_firmware1.png
                :width: 6.5in
                :height: 4.0in

        2. Click the red circled "2" to open the firmware flashing dialog, change the start address to "0x8010000", and browse to load the downloaded OpenRTK330 firmware bin file, then click "Start"

            .. figure:: media/st-link_utility_flash_firmware2.png
                :width: 6.5in
                :height: 4.0in

#. **Check** the **LED** indicator: there are Yellow, Red and Green three LED lights on the OpenRTK330 EVB, if the firmware is loaded correctly, the Yellow LED is flashing first, indicating the 1PPS signal from ST GNSS chipset is available. Then, connect the SMA female connector with a satellite antenna (OpenRTK330 EVB can power on the antenna if passive, otherwise use a DC blocker), the Green LED starts flashing, indicating the OpenRTK330 INS App is running with valid GNSS signal. At this point, the firmware is loaded completely.

..
    At this point, the OpenRTK330 firmware is loaded and ready for GNSS RTK positioning that also requires internet connection to a NTRIP server for GNSS data correction.  and then connects with Aceinna's OpenRTK Android App for internet connectivity (see next section). Alternatively, the following step can be performed to get internet connectivity

..
    (optional) Connect the EVB (RJ45 connector) with a network router/gateway with an Ethernet cable, the usage of this connection will also be addressed in next section


EVK Vehicle Installation
~~~~~~~~~~~~~~~~~~~~~~~~
 

Troubleshooting
~~~~~~~~~~~~~~~~~~~~~~~


Note
~~~~~~~~~~~~~~~~~~~~~~~
The following section elaborate on Aceinna's Cloud Service on Cloud RTK, GNSS base station network and NTRIP server, followed by the section describes two types of user interface to use OpenRTK330 EVK for GNSS/INS real time positioning.
