PC
===

Using OpenRTK330 with a PC needs to access the web based Aceinna developer website via ethernet connection.

Usage
~~~~~~~~~~~~~
1. Power on the OpenRTK330 EVB using a 9-12 v DC adaptor or a microUSB cable connected to a PC, then connect the EVB with a GNSS antenna, and lastly check the YELLOW, RED AND GREEN LED lights to confirm valid firmware

  - YELLOW: flashing light indicating GNSS chipsets is powered on with valid 1PPS signal output
  - GREEN: flashing light indicating OpenRTK330 RTK or INS App is running correctly with valid GNSS signal receiving 

2. **Connection**

 - Plug in a RJ45 cable connected with the same router as the PC to the ethernet port of OpenRTK330 EVB. By factory setting, an IP address is allocated to OpenRTK330 device in the way of DHCP. Make sure the ligths on the RJ45 interface are flashing, which means valid ethernet connection. 

 - Run the python driver for OpenRTK330 to get connectted with Aceinna developer website. There are two options

    - Download executable files (version 1.1.0) and run in a command line          

        - `Windows 10 <https://github.com/Aceinna/python-openimu/files/4211970/ans-devices-win.zip>`_

        - `Mac OS <https://github.com/Aceinna/python-openimu/files/4211966/ans-devices-mac.zip>`_

        - `Linux (Ubuntu 19.10) <https://github.com/Aceinna/python-openimu/files/4211966/ans-devices-mac.zip>`_

        - `Raspberry Pi (Raspbian GNU/Linux 9) <https://github.com/Aceinna/python-openimu/files/4211966/ans-devices-mac.zip>`_

    - If you prefer building from source, go to Aceinna's github page and clone the repo `python-openimu <https://github.com/Aceinna/python-openimu>`_, and checkout the "ans-devices" branch. Run the OpenRTK Python driver with the following commands:

            .. code-block:: python

                cd .\python-openimu
                pip install -r requirements.txt
                python main.py

    The python driver automatically scans available USB-serial ports and finds the right com port that reports positioning information packets. If ethernet connection is established successfully via python driver, the "WS Server connected" notification appears and the "Play" button is highlighted on `OpenRTK Monitor <https://developers.aceinna.com/devices/rtk>`_, as shown below

     .. image:: ../media/web_gui_connect.png
       :align: center

3. **Map Presentation**

 - Click "Play", Device information is exposed on the DEVICE INFO page (https://developers.aceinna.com/devices/rtk). 

 .. image:: ../media/web_gui_play.png
   :align: center
   :scale: 50%


4. **NTRIP and User Configurations**

 A lightweight TCP/IP web service is started inside the OpenRTK330 firmware after power on, user can get access to the web service via a simple `web GUI <http://openrtk>`_ (http://openrtk)

    

    .. manually setup a STATIC IP (ip = 192.168.1.110, netmask =  255.255.255.0, gateway = 192.168.1.1).

            **Generate API**. If you do not have the API key, you need `generate API
         ``key <https://openrtk.readthedocs.io/en/latest/Network/getapikey.html>`__  
         to use Aceinna RTK network and set the number of allowed devices.

         .. image:: ../media/signup.png
            :align: center
            :scale: 50%

- On the home page, user can change and save NTRIP settings of user's GNSS correction data provider. 

  - "NTRIP STATUS" field with "CONNECTED" string indicates that the ethernet connection to NTRIP server is valid 
  - "BASE STREAM" field with string value "AVAILABLE" indicates GNSS correction data stream from NTRIP server is valid. 

    .. image:: ../media/ntripconfi.png
             :align: center
             :scale: 50%
 

- On the User Configuration page, user can change and save OpenRTK330 device settings, like lever arm, output packet and so on.

    .. image:: ../media/usercfg.png
       :align: center
       :scale: 50%

 - Ethernet Configuration

  - DHCP: factory setting 
  - STATIC IP: STATIC NETMASK and STATIC GATEWAY. You should config the same gateway as your network equipment and choose an available IP address.

  You can click 'SAVE' to let the configuration be effective immediately. If the NTRIP STATUS is CONNECTED, it will reconnect immediately.

    .. image:: ../media/ethcfg.png
       :align: center
       :scale: 50%

- Device Info page: user can see all the product infomation of the connected device.

    .. image:: ../media/deviceinfo.png
       :align: center
       :scale: 50%

5. **Data Logging**
 * If the aforemetioned python driver for OpenRTK330 is running on your PC or Raspberry Pi, a "data" folder is generated under the path of the command line and the following binary files are logged inside this foder

    - user_<time>.bin: USER com port output
    - debug_<time>.bin: DEBUG com port output
    - rtcm_<time>.bin: GNSS RTCM com port output 

 * Run the following python script (requires clone of the github repo `python-openimu <https://github.com/Aceinna/python-openimu>`_) to parse the OpenRTK330 binary files

     .. code-block:: python

                    cd .\python-openimu\openrtk_data_parse
                    python openrtk_parse.py -p <file path>

   A few decoded "csv" files are generated from the "user_<time>.bin" and "debug_<time>.bin" output, each of the "csv" contents are described in its file header. 

