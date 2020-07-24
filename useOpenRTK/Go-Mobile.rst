With an Android Smartphone
============================

Using the OpenRTK330L EVK to evaluate the module requires

* the installation the "OpenRTK" Android App: provides 4G access to NTRIP server over the internet
* Micro-USB connection to a PC for power and data logging connection

"OpenRTK" App Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Scan the QR code below or click `here <https://developers.aceinna.com/static/appDownload.html/>`_ to download the Android apk installation file. Make sure your Android version is 8.0 or above.

 .. image:: ../media/ercode.png
    :align: center
    :scale: 70%


2. Open the downloaded APK file to install the App 

 .. note::

     Please grant the OpenRTK App access to run in Android system's backend.

Usage
~~~~~~~~~~~~~

1. **Connection**
  * Connect the OpenRTK330 EVB to a PC via a Micro-USB cable, then connect the EVB with a GNSS antenna, checking the LED lights for working status

  - YELLOW: flashing light indicating GNSS chipsets is powered on with valid 1PPS signal output
  - GREEN: flashing light indicating OpenRTK330L INS App is running correctly with valid GNSS signal receiving 

  * Enable "Bluetooth" function and "Location" access right for "OpenRTK" App on your Anroid device 
  * Open the "OpenRTK" Andorid App, as shown by the picture below, go to the "Connect" tab and click the "search" icon (right bottom) to search for your device. If your OpenRTK330 device is found, a Bluetooth device ID appears on the "Connect" list. By factory setting, the Bluetooth device ID is "OpenRTK_<four digits>" and the four digits are the last four digits of your OpenRTK330 module S/N. Click your Bluetooth device ID and if connected successfully, a notification appears

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

6. **Data Logging and Parsing**
 
 * **Logging**: Download the latest version of Python driver executable (click `here <https://github.com/Aceinna/python-openimu/releases>`_), unzip the file, and run the following command, e.g. on Windows 10

    .. code-block:: bash

          C:\pythondriver-win\ans-devices.exe 

    The running Python driver automatically logs all UART output from OpenRTK330L module. A "data" folder is created inside the Python driver folder and a "log file" folder is created inside the "data" foder. Each "log file" folder includes the following files:

    - *configuration.txt*: the parameter settings of the current module

    - *user_<time>.bin*: USER com port output
      
      - RAWDATA App: raw IMU data in "s1" packet format
      - RTK App: GNSS RTK solution in "sK" and "pS" packets
      - RTK_INS App: GNSS RTK and INS integraed solution in "sK" and "pS" packets
    - *debug_<time>.bin*: DEBUG com port output

      - RAWDATA App: N/A or base GNSS RTCM data if you configured a NTRIP server with RTCM correction, in this case, the output bin file is named *rtcm_base_<time>.bin* 
      - RTK App: N/A
      - RTK_INS App: GNSS RTK and INS integraed solution in "p1" packets
    - *rtcm_rover_<time>.bin*: GNSS RTCM com port output 


 * **Parsing**: Run the following python script inside the Python driver folder to parse the logged OpenRTK330L binary files

     .. code-block:: python

          cd C:\pythondriver-win\openrtk_data_parse
          python openrtk_parse.py -p ..\data\<OpenRTK data log folder>

    A few "csv" files are decocded from the "user_<time>.bin" and "debug_<time>.bin" output, the content of each of the "csv" files is described in its file header. 

    (Optional) On Windows 10, download `convbin.exe <https://virtualmachinesdiag817.blob.core.windows.net/tools/convbin.exe>`_ and run the program to decode the logged GNSS RTCM binary files to obtain `RINEX <https://www.igscb.org/wg/rinex/>`_ text files for quick checking.


