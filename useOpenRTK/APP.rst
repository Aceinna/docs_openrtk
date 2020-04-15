Android
=======

The following describes the installation and usage of the "OpenRTK" Android App.

Installation
~~~~~~~~~~~~~~~~~

1. Scan the QR code below or click `here <https://developers.aceinna.com/static/appDownload.html/>`_ to download the Android apk installation file. Make sure your Android version is 8.0 or above.

 .. image:: ../media/ercode.png
    :align: center
    :scale: 70%


2. Open the downloaded APK file to install the App 

 .. note::

     Please grant the OpenRTK App access to run in Android system's backend.

Usage
~~~~~~~~~~~~~
1. Power on the OpenRTK330 EVB using a 9-12 v DC adaptor or a micro-USB cable to a powered USB outlet, then connect the EVB with a GNSS antenna, and lastly check the YELLOW, RED AND GREEN LED lights to confirm valid firmware

  - YELLOW: flashing light indicating GNSS chipsets is powered on with valid 1PPS signal output
  - GREEN: flashing light indicating OpenRTK330 RTK or INS App is running correctly with valid GNSS signal receiving 

2. **Connection**

 - Enable "Bluetooth" on your Anroid device
 - Open the OpenRTK Andorid App and enable "Location" access for "OpenRTK" App on your Anroid device 
 - As shown by the picture below, go to the "Connect" tab and click the "search" icon (right bottom) to search for your device. If your OpenRTK330 device is found, a Bluetooth device ID appears on the "Connect" list. By factory setting, the Bluetooth device ID is "OpenRTK_<four digits>" and the four digits are the last four digits of your OpenRTK330 module S/N. Click your Bluetooth device ID and if connected successfully, a notification appears

  .. image:: ../media/connect_success.jpeg
    :align: center
    :scale: 18%   

 - Besides, detailed Bluetooth connection and user configuration information of the device can be found on the lower window of the "Log" tab, and NMEA GGA messages are reporting the OpenRTK330 device position on the upper window of the "Log" tab. 

  .. image:: ../media/connectLog.jpg
    :align: center
    :scale: 18%   

3. **Map Presentation**

 - Once the Bluetooth connection made successfully, and OpenRTK330 is reporting positioning information to Android App, go to "Map" tab and click "Start Live Data" to start a live map presentation 

  .. image:: ../media/live_map_start.jpg
    :align: center
    :scale: 18%   

 - Real time positioning information and trajectory is shown 

  .. image:: ../media/live_map.jpg
    :align: center
    :scale: 18%  

4. **NTRIP Configuration**

 - In order to get GNSS RTK positioning, go to "NTRIP" tab and configure the NTRIP server settings of your GNSS correction data provider 

    .. image:: ../media/ntrip_config.jpeg
       :align: center
       :scale: 18%

 - Click "SAVE" to save your NTRIP server settings to your OpenRTK330 module, and then switch on "Pull Base" to get GNSS correction data for RTK  

    .. image:: ../media/ntrip_success.jpeg
       :align: center
       :scale: 18%
      

5. **User Configuration**.

  From anyone of the four tabs, you can access the menu for user configuration by clicking the icon "≡" at the upper left corner 

  .. image:: ../media/leftMenu.png
       :align: center
       :scale: 18%

  - Click "Device Advanced": user can change and save OpenRTK330 device settings, like Bluetooth ID, lever arm and so on.
    
    .. image:: ../media/customDeviceConfig.jpg
         :align: center
         :scale: 18%

  - Click "Developer Option": user can configure the Android App on map presentation and switch on/off of saving positioning results (NMEA GGA messages only) to Android phone storage. The defualt storage path is "Android/data/com.aceinna.rtk/files/log"   

    .. image:: ../media/android_app_config.jpeg
         :align: center
         :scale: 18%

6. **Data Logging**
 * To log OpenRTK330 com port output when using the Anroid App, you have to connect OpenRTK330 with a PC or Raspberry Pi via micro-USB and run the aforementioned python driver on your PC or Raspberry Pi 

   - either

     .. code-block:: bash

            ./ans-devices -r

   - or 

     .. code-block:: python

            cd ./python-openimu/
            python main.py -r

   where "-r" means raw data logging. A "data" folder is generated under the path of the command line and the following binary files are logged inside this foder. The contents of "USER" and "DEBUG" com port output are different between Apps

   - *user_<time>.bin*: USER com port output
      
     - RAWDATA App: 100 Hz raw IMU data in "s1" packet format
     - RTK App: GNSS RTK solution in "sR" and "pS" packets
     - RTK_INS App: GNSS RTK and INS integraed solution in "sR" and "pS" packets
   - *debug_<time>.bin*: DEBUG com port output

     - RAWDATA App: N/A or base GNSS RTCM data if you configured a NTRIP server with RTCM correction, in this case, the output bin file is named *rtcm_base_<time>.bin* 
     - RTK App: GNSS RTK solution in "k1" packets
     - RTK_INS App: GNSS RTK and INS integraed solution in "p1" packets
   - *rtcm_rover_<time>.bin*: GNSS RTCM com port output 


 * Run the following python script (requires clone of the github repo `python-openimu <https://github.com/Aceinna/python-openimu>`_) to parse the OpenRTK330 binary files

     .. code-block:: python

                    cd .\python-openimu\openrtk_data_parse
                    python openrtk_parse.py -p <file path>

   A few decoded "csv" files are generated from the "user_<time>.bin" and "debug_<time>.bin" output, the content of each of the "csv" files is described in its file header. 


 * (Optional) On Windows 10, download `convbin.exe <https://virtualmachinesdiag817.blob.core.windows.net/tools/convbin.exe>`_ and run the program to decode the logged GNSS RTCM binary files to obtain `RINEX <https://www.igscb.org/wg/rinex/>`_ text files for viewing.


