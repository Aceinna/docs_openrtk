Firmware Online Upgrade
=================================

.. contents:: Contents
    :local:


WARNING!!!
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
                    :width: 5.5in
                    :height: 3.5in
   
   5. Click Target->Option Bytes and select "sector 0", "sector 1", "sector 2", "sector 3" and "sector 11" to perform write protection. Click Apply button for make it effective. 

     .. figure:: media/protect_sections.png
                    :width: 5.5in
                    :height: 9.5in    


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

          .\AceinnaGnssTool.exe program -f t5 -iÂ C:\GNSS_RTK_SDK.binÂ -o log.txt -c COM6Â -b 115200 -m SQI -e TRUE -r TRUE

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
  
  