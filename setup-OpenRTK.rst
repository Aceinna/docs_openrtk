OpenRTK330 EVK and Setup
=================================

.. contents:: Contents
    :local:

EVK Introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The OpenRTK330 Evalution Kit (EVK) is designed to evaluate the OpenRTK330 module with the  online Aceinna Navigation Studio (ANS) and related software stack. A full set of OpenRTK330 EVK is shown below after you unpack the product box. 

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

The ESP32 bluetooth module on the OpenRTK330 EVB has been programmed and configured to provide bluetooth wireless connectivity, and user do not have to get hands on it.

The OpenRTK330 EVK is all set as a high precision GNSS/INS positioning platform before shipping out, you could skip the following firmware installation/update process and directly go to learn `How to Use OpenRTK330 EVK <https://openrtk.readthedocs.io/en/latest/useOpenRTK.html>`_. Otherwise, if you want to update the module with the latest firmware, follow the instructions below carefully.


Firmware Installation/Update
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**WARNING**: Checkout **Troubleshooting** #1 at the end of this page, before you perform any firmware operation.

.. 2. **Connect** the OpenRTK330 EVB to a PC via a Micro-USB cable, four serial ports are established on your PC as shown below (e.g. on Windows 10), meanwhile the EVB is powered up by this USB connection. In the context of this manual, we refer "COM3" to the FIRST serial port and refer the other three serial ports to the SECOND, THRID and FOURTH serial port in increasing order.


.. Alternatively, the EVB can be powered up directly by a 9-12v DC adapter/generator. In this case, the USB connection is just a data link. The LED beside the Micro-USB port on the EVB is always on if powered up.



Go to the online App Center of ANS (click `here <https://developers.aceinna.com/code/apps>`_) to **install/update** the OpenRTK330 module **firmware**, as shown below
  
  .. figure:: media/download_openrtk330_firmware.png
          :width: 7.0in
          :height: 3.0in

  There are two major steps to install/update OpenRTK330 firmware:
  
    1. Firstly, install/update "GNSS firmware" (Windows 10 only)

      * Download the flashing tool ("AceinnaGnssTool.exe") from `here <https://virtualmachinesdiag817.blob.core.windows.net/tools/AceinnaGnssTool.exe>`_
      * Click the "GNSS_RTK_SDK" App and click the "Download" button to download the App bin file to your PC, store it in, e.g. *C:\GNSS_RTK_SDK.bin*
      * Put the "boot mode switch" (#7 in the EVB picture above) in position **"A"**
      * Power on the EVB via connecting a Micro-USB cable between the EVB and your PC, only the GREEN LED keeps blinking. There are four serial com ports established on the PC, as shown by the example below, "COM3" refers to the first and "COM6" refers to the fourth serial port

        .. figure:: media/FourSerialPorts.PNG
          :width: 5.0in
          :height: 2.0in

      * Run the command below to flash the firmware, and change both the path to the firmware bin file and the com port number (use the fourth serial port)

        .. code-block:: python

          .\AceinnaGnssTool.exe program -f t5 -i C:\GNSS_RTK_SDK.bin -o log.txt -c COM6 -b 115200 -m SQI -e TRUE -r TRUE

      * Check the output file "log.txt" in the same directory of the executable file, if the below log is found at the end of the "log.txt" file, firmware flasing is done successfully

        .. code-block:: python

          Program OK: instance 0


    2. Then, install/update OpenRTK330 App (supports Windows 10, Mac OS, Ubuntu and Raspberry Pi platforms) 
    
      * Put the "boot mode switch" (#7 in the EVB picture above) in position **"B"**
      * Power (Re-power) on the EVB via connecting a Micro-USB cable between the EVB and your PC, the YELLOW LED keeps blinking
      * Download the excutable OpenRTK/OpenIMU python driver (version 1.6.0) from `here <https://github.com/Aceinna/python-openimu/releases>`_ , and run in a command line          

          .. code-block:: bash

              ./ans-devices

        - (Optional) The OpenRTK/OpenIMU python driver is open sourced on GitHub, if you prefer building from source, clone the repo from `here <https://github.com/Aceinna/python-openimu>`_, and checkout the "master" branch. Install and run it with the following commands:

            .. code-block:: python

                cd .\python-openimu
                pip install -r requirements.txt
                python main.py

        The python driver keeps scanning available serial ports to find the correct one for OpenRTK/OpenIMU, if found, you will see the following console output

          .. figure:: media/python_driver_connects.PNG
            :width: 6.0in
            :height: 1.0in

      * On the above App Center webpage, click "RTK_INS" App, If the correct com port is found by the python driver, the "UPGRADE" button circled by cyan rectrangle will be highlighted. Then click the "UPGRADE" button to start the App update process, and only the GREEN LED keeps blinking during the process. 
      
        .. figure:: media/app_upgrade.png
            :width: 6.5in
            :height: 4.0in

      * Upon finishing, you will see the dialog below on the App Center webpage, indicating successful App update. The GREEN LED stops blinking fast, and the YELLOW LED starts to blink first then the GREEN LED is lighted. The blinking YELLOW LED indicates ST GNSS chipset is powered on and working properly, and blinking GREEN LED indicates App is running properly with valid GNSS signal receiving and valid GNSS antenna connection. Note GREEN LED stays on if no antenna is connected.

        .. figure:: media/App_Upgrade_Suc.PNG
            :width: 6.5in
            :height: 4.0in
      
.. Then, connect the SMA female connector with a satellite antenna (OpenRTK330 EVB can power on the antenna if passive, otherwise use a DC blocker), the Green LED starts flashing, indicating the OpenRTK330 INS App is running with valid GNSS signal. At this point, the firmware is loaded completely.

.. At this point, the OpenRTK330 firmware is loaded and ready for GNSS RTK positioning that also requires internet connection to a NTRIP server for GNSS data correction.  and then connects with Aceinna's OpenRTK Android App for internet connectivity (see next section). Alternatively, the following step can be performed to get internet connectivity

.. (optional) Connect the EVB (RJ45 connector) with a network router/gateway with an Ethernet cable, the usage of this connection will also be addressed in next section

Firmware Options
~~~~~~~~~~~~~~~~~
The previous section demonstrates the firmware installation process for OpenRTK330 with "RTK_INS" App as an example. In order to fullfill various user requirements, there are a few firmware options provided with OpenRTK330, as listed on the online `App center <https://developers.aceinna.com/code/apps>`_. The following are introductions on these Apps:

  * RAWDATA APP - without GNSS or INS algorithm

    * 10 Hz raw GNSS data output in RTCMv3 format
    * Configurable rate (50, 100, and 200 Hz) of raw IMU data output in binary format
    * Logging the raw data to file, refer to `How to Use OpenRTK330 <https://openrtk.readthedocs.io/en/latest/useOpenRTK.html>`_
    * Embedding your own RTK/INS algorithms, refer to `Firmware Build from Source <https://openrtk.readthedocs.io/en/latest/build_firmware.html>`_ 


  * RTK APP - with GNSS RTK algorithm

    * 10 Hz raw GNSS data output in RTCMv3 format
    * Configurable rate (50, 100, and 200 Hz) of raw IMU data output 
    * GNSS RTK position, velocity and accuracy metrics output
    * Logging the raw data and RTK solution to file, refer to `How to Use OpenRTK330 <https://openrtk.readthedocs.io/en/latest/useOpenRTK.html>`_

  * RTK_INS APP - with GNSS RTK and INS integrated algorithm

    * 10 Hz raw GNSS data output in RTCMv3 format
    * Configurable rate (50, 100, and 200 Hz) of raw IMU data output 
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


