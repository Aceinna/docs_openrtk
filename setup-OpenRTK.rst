OpenRTK330 EVK Setup
=================================

.. contents:: Contents
    :local:

The OpenRTK330 Evalution Kit (EVK) is designed to evaluate the performance of the GNSS/INS positioning with OpenRTK330 module and the OpenRTK software stack. The major components of the OpenRTK330 EVK are shown by the figure below, followed by steps to quickly setup the EVK and install the firmware on the OpenRTK330 module. 

.. figure:: media/EvalKit.png
    :width: 6.0in
    :height: 6.0in

Firmware Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. **Unpack** the OpenRTK330 EVK, you will find the following **items** for the EVK quick setup and firmware installation

    * OpenRTK330 Evaluation Board (EVB)
    * ST-Link Debugger
    * Micro USB cable
    * Mounting plate

    By factory settings, the ESP32 module on the OpenRTK330 EVB has been programmed and configured to provide Bluetooth wireless connectivity. The Ethernet and CAN drivers are also set.

2. **Mount** the EVB on the mounting plate for easy operation.

3. **Connect** the micro USB port on the OpenRTK330 EVB to a PC via a USB cable, four serial ports will be established on your PC, and the EVB is also powered up by this USB connection. Alternatively, the EVB can be powered up directly by a 9-12v DC adapter/generator. In this case, the USB connection is just a data link. The red LED by the micro-USB is always on if powered up.

4. **Install** the OpenRTK330 module **firmware** from `App center <https://developers.aceinna.com/code/apps>`_ on Aceinna's developer website, as shown by

    .. figure:: media/download_openrtk330_firmware.png
        :width: 6.5in
        :height: 3.0in

  Taking the "RTK_INS" App as an example, click it, and there appears **two options** to install the OpenRTK330 firmware online:

        .. figure:: media/app_upgrade.png
            :width: 6.5in
            :height: 4.0in

  a. (Recommended) The **online upgrade** way  

    - Run the python driver for OpenRTK330 to get connectted with Aceinna developer website. There are two options

      - Download executable files (version 1.6.0) of python driver 

        - `Windows 10 <https://github.com/Aceinna/python-openimu/files/4626456/develop-win.zip>`_

        - `Mac OS <https://github.com/Aceinna/python-openimu/files/4626462/develop-mac.zip>`_

        - `Linux (Ubuntu 19.10) <https://github.com/Aceinna/python-openimu/files/4626464/develop-ubuntu.zip>`_

        .. - `Raspberry Pi (Raspbian GNU/Linux 9) <https://github.com/Aceinna/python-openimu/files/4449019/develop-rpi.zip>`_ 

        and run in a command line          

            .. code-block:: bash

                ./ans-devices

      - If you prefer building from source, go to Aceinna's github page and clone the repo `python-openimu <https://github.com/Aceinna/python-openimu>`_, and checkout the "master" branch. Run the OpenRTK Python driver with the following commands:

            .. code-block:: python

                cd .\python-openimu
                pip install -r requirements.txt
                python main.py

        The python driver automatically scans available USB-serial ports and finds the right com port. If the correct com port is found, the "UPGRADE" button circled by cyan rectrangle will be highlighted. Then click the "UPGRADE" button to start the firmware upgrade process and wait it completes.  

  b. (**WARNING!** Checkout **Troubleshooting** #1 at the end of this page, before you go down this path) **Download** the firmware bin file and **flash** it into OpenRTK330 module. In order to fullfill this, first install the STM32 ST-LINK Utility software from `here <https://www.st.com/en/development-tools/stsw-link004.html>`_ on your PC. Then open the STM32 ST-LINK Utility software and connect the OpenRTK330 EVB with PC using the ST-LINK debugger,

    1. Click the red circled "1" to establish a connection with the OpenRTK EVB

            .. figure:: media/st-link_utility_flash_firmware1.png
                :width: 6.5in
                :height: 4.0in

    2. Click the red circled "2" to open the firmware flashing dialog, change the start address to "0x8010000", and browse to load the downloaded OpenRTK330 firmware bin file, then click "Start"

            .. figure:: media/st-link_utility_flash_firmware2.png
                :width: 6.5in
                :height: 4.0in

5. **Check** the **LED** indicator: there are Yellow, Red and Green three LED lights on the OpenRTK330 EVB, if the firmware is loaded correctly, the Yellow LED is flashing first, indicating the 1PPS signal from ST GNSS chipset is available. Then, connect the SMA female connector with a satellite antenna (OpenRTK330 EVB can power on the antenna if passive, otherwise use a DC blocker), the Green LED starts flashing, indicating the OpenRTK330 INS App is running with valid GNSS signal. At this point, the firmware is loaded completely.

..
    At this point, the OpenRTK330 firmware is loaded and ready for GNSS RTK positioning that also requires internet connection to a NTRIP server for GNSS data correction.  and then connects with Aceinna's OpenRTK Android App for internet connectivity (see next section). Alternatively, the following step can be performed to get internet connectivity

..
    (optional) Connect the EVB (RJ45 connector) with a network router/gateway with an Ethernet cable, the usage of this connection will also be addressed in next section

Firmware Options
~~~~~~~~~~~~~~~~~
The previous section demonstrates the firmware installation process for OpenRTK330 with "RTK_INS" App as an example. In order to fullfill various user requirements, there are a few firmware options provided with OpenRTK330, as listed on the `App center <https://developers.aceinna.com/code/apps>`_. The following are introductions on these Apps:

  * RAWDATA APP - without GNSS or INS algorithm

    * 10 Hz raw GNSS data output in RTCM format
    * 100 Hz raw IMU data output in binary format
    * Logging the raw data to file, refer to `How to Use OpenRTK330 <https://openrtk.readthedocs.io/en/latest/useOpenRTK.html>`_
    * Embedding your own RTK/INS algorithms, refer to `Firmware Build from Source <https://openrtk.readthedocs.io/en/latest/build_firmware.html>`_ 

  * RTK APP - with GNSS RTK algorithm

    * 10 Hz raw GNSS data output in RTCM format
    * 100 Hz raw IMU data output 
    * GNSS RTK position, velocity and accuracy metrics output
    * Logging the raw data and RTK solution to file, refer to `How to Use OpenRTK330 <https://openrtk.readthedocs.io/en/latest/useOpenRTK.html>`_

  * RTK_INS APP - with GNSS RTK and INS integrated algorithm

    * 10 Hz raw GNSS data output in RTCM format
    * 100 Hz raw IMU data output 
    * INS/GNSS RTK integrated solution output, include position, velocity and attitude and accuracy metrics
    * Logging the raw data and INS solution to file, refer to `How to Use OpenRTK330 <https://openrtk.readthedocs.io/en/latest/useOpenRTK.html>`_

  .. * DEMO APP - GNSS RTK playback

EVB Vehicle Installation
~~~~~~~~~~~~~~~~~~~~~~~~
In order to install the OpenRTK330 EVB on vehicle for driving test, a few reference frames listed below has to be defined  

 * **The IMU body frame** is defined as below in the figure, and by default the INS solution of OpenRTK330 is provided at the center of navigation of the IMU.

    .. figure:: media/imu_body_xyz.jpeg
        :width: 5.0 in
        :height: 5.0 in
   
 * **The vehicle frame** is defined as

   * x-axis: points out the front of the vehicle in the driving direction
   * z-axis: points down to the ground
   * y-axis: completes the right-handed system
 * **The local level navigation frame** is defined as

   * x-axis: points north 
   * z-axis: points down parallel with local gravity
   * y-axis: points east 
 * **The user output frame** is used to transfer the INS solution to a user designated position.

Depends on the vehicle installation of the OpenRTK330 system, user has to configure two types of offsets to make the GNSS integrated INS solution work
 
 * Translation offset
   
   * *GNSS antenna lever-arm*: GNSS position is estimated to the phase center of the GNSS antenna, and INS position is estimated to the center of the navigation of the IMU. The translation from the IMU center to the phase center of the GNSS antenna has to be known and applied to the integrated system via user configuration of the antenna lever-arm. The GNSS/INS integrated solution outputs position at the IMU center.
   * *User output lever-arm*: If user wants the above GNSS/INS integrated solution output at a more useful position, the translation between the IMU center and the designated point of interest has to be known and applied via user configuration of point of interest lever-arm.
 * Rotation offset: If the axes of the IMU body frame of the installed OpenRTK330 unit is not aligned with the vehicle frame, the orientation of the IMU relative to the vehicle also has to be known and applied via user configuration of rotation angles between the IMU body frame and vehicle frame. 

Please refer to `How to Use OpenRTK330 <https://openrtk.readthedocs.io/en/latest/useOpenRTK.html>`_ section of this manual to carry out the user configuration operations through the Anroid App and Web GUI.


Troubleshooting
~~~~~~~~~~~~~~~~~~~~~~~
I. **SAVE BEFORE DEVELOPMENT START**: it's strongly recommended to save your factory OpenRTK330 module system image file to a binary file to be able to recover the whole system if something unexpected happened! Especially, if the system bootloader and IMU calibration tables are damaged, OpenRTK330 will not work properly.

 - Save system image

   1. Download and install ST-Link Utility from `here <https://www.st.com/en/development-tools/stsw-link004.html>`_
   2. Connect ST-Link debugger between OpenRTK330 EVB and PC and power on the EVB
   3. Open ST-Link Utility software on the PC and go to Target->Connect
   4. Enter value 0x100000 in Size bo and hit enter
   5. Click File->Save As to save the system image file

    .. figure:: media/save_image.png
                :width: 6.5in
                :height: 4.0in

 - Recover system image

   1. Connect ST-Link debugger between OpenRTK330 EVB and PC and power on the EVB
   2. Open ST-Link Utility software on the PC and go to Target->Connect
   3. Click File->Open and open previously saved image file
   4. Click Target->Program & Verify and make sure that the start address is 0x08000000 before you click Start button to re-programming the OpenRTK330 module

    .. figure:: media/re-download_image.png
                    :width: 6.5in
                    :height: 4.0in
   
   5. Click Target->Option Bytes and select "sector 0", "sector 1", "sector 2", "sector 3" and "sector 11" to perform write protection. Click Apply button for make it effective. 

     .. figure:: media/protect_sections.png
                    :width: 6.5in
                    :height: 11.0in


