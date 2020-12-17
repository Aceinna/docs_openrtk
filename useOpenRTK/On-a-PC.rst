With a PC
===========

Using the OpenRTK330L EVK to evaluate the OpenRTK330L product with a PC requires 

 * access to the online web based Aceinna Navigation Studio (ANS) via the Micro-USB connection between the EVB and the PC
 * access to NTRIP server over the internet via a Ethernet connection between the EVB and the PC


Usage 
~~~~~~~~~~~~~

1. **Connection**

  - Connect the OpenRTK330 EVB to a PC via a Micro-USB cable, then connect the EVB with a GNSS antenna, checking the LED lights for working status

    - YELLOW: flashing light indicating GNSS chipsets is powered on with valid 1PPS signal output
    - GREEN: flashing light indicating OpenRTK330L INS App is running correctly with valid GNSS signal receiving from the antenna connection

  - Download the latest version of Python driver executable (click `here <https://github.com/Aceinna/python-openimu/releases>`_), unzip the file, and run the following command, e.g. on Windows 10

    .. code-block:: bash

          C:\pythondriver-win\ans-devices.exe

  .. - If you prefer building from source, go to Aceinna's github page and clone the repo `python-openimu <https://github.com/Aceinna/python-openimu>`_, and checkout the "master" branch. Run the OpenRTK Python driver with the following commands:

     ..     .. code-block:: python

       ..       cd .\python-openimu
         ..     pip install -r requirements.txt
           ..   python main.py

  - The Python driver automatically establishes connection with the ANS web GUI. If succedded, the "WS Server connected" notification appears and the "Play" button is highlighted on `OpenRTK Monitor <https://developers.aceinna.com/devices/rtk>`_, as shown below

     .. image:: ../media/web_gui_connect.png
       :align: center

  - Use RJ45 cables to connect the EVB and the PC (PC could use WiFi if within the same local subnetwork) to the **SAME** router/network switch, and by factory setting, an IP address is allocated to OpenRTK330 device in the way of DHCP. Make sure the ligths on the RJ45 interface are flashing, which means valid ethernet connection. 

2. **Map Presentation**

  Click "Play", you will find a live map showing the position trajectory, a navigation information display panel, and two panels for satellite information on SNR, azimuth, and elevation angles. In addtion, on the most right side, click "chart" icon you will get a satellite Skyview Chart.

    .. image:: ../media/web_gui_play.png
      :align: center
      :scale: 50%


3. **NTRIP and User Configurations**

  A lightweight TCP/IP web service is started inside the OpenRTK330 firmware after power on, then user can get access to ANS `web GUI <http://openrtk>`_ (http://openrtk) via the Ethernet connection

    .. manually setup a STATIC IP (ip = 192.168.137.110, netmask =  255.255.255.0, gateway = 192.168.137.1).

            **Generate API**. If you do not have the API key, you need `generate API
         ``key <https://openrtk.readthedocs.io/en/latest/Network/getapikey.html>`__  
         to use Aceinna RTK network and set the number of allowed devices.

         .. image:: ../media/signup.png
            :align: center
            :scale: 50%

    - On the home page, user can change and save NTRIP settings of user's GNSS correction data provider. 

      - "NTRIP STATUS" field with "CONNECTED" string indicates that the ethernet connection to NTRIP server is valid 
      - "BASE STREAM" field with string value "AVAILABLE" indicates GNSS correction data stream from NTRIP server is valid. 

        .. image:: ../media/ntrip_config.png
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

    ..   The other way is to connect the EVB and the PC directly, which requires network sharing between the PC and the EVB. For example, with a Windows 10 PC, 

    .. * Go to Control Panel\Network and Internet\Network Connections, an Ethernet subnetwork is established for the Ethernet connection between the EVB and the PC, e.g. “Ethernet 2” as shown below. 
    
    ..   .. figure:: media/network_connections.png
    ..     :width: 6.5in
    ..     :height: 3.0in
    
    .. * Right-click “Ethernet 2”, and then click “Properties”, on the “Networking” tab, click “Internet Protocol Version 4 (TCP/IPv4)”, configure the IP settings as follows: the gateway has to be 192.168.137.1, and the subnet mask has to be 255.255.255.0, while the IP address can be assigned to one that has not been taken in the network 192.168.137.xx.

    ..     .. figure:: media/network_setting_eth.png
    ..       :width: 6.0in
    ..       :height: 3.5in

    .. * Then, right-click WLAN (assuming the PC uses WiFi for internet access), go to Properties->Sharing, check the “Allow other network users to connect through this computer’s internet connection”, and select “Ethernet 2” on the drop down menu below, click “OK” to enable the EVB to have access to internet shared by the PC. 

    ..     .. image:: media/network_sharing.png
    ..         :align: center
    ..         :scale: 50%


    - Device Info page: user can see all the product information of the connected device.

      .. image:: ../media/deviceinfo.png
         :align: center
         :scale: 50%

    

4. **Data Logging and Parsing**

  - Using the UART

    * **Logging**: Download the latest version of Python driver executable (click `here <https://github.com/Aceinna/python-openimu/releases>`_), unzip the file, and run the following command, e.g. on Windows 10

      .. code-block:: python

            C:\pythondriver-win\ans-devices.exe 

      The running Python driver automatically logs all UART output from OpenRTK330L module. A "data" folder is created inside the Python driver folder and a "log file" folder is created inside the "data" foder. Each "log file" folder includes the following files:

      - *configuration.txt*: the parameter settings of the current module

      - *user_<time>.bin*: USER com port output
        
        - RAWDATA App: raw IMU data in "s1" packet format
        - RTK_INS App: GNSS RTK and INS integraed solution in "sK" and "pS" packets
      - *debug_<time>.bin*: DEBUG com port output

        - RAWDATA App: N/A or base GNSS RTCM data if you configured a NTRIP server with RTCM correction, in this case, the output bin file is named *rtcm_base_<time>.bin* 
        - RTK_INS App: GNSS RTK and INS integraed solution in "p1" packets
      - *rtcm_rover_<time>.bin*: GNSS RTCM com port output 


    * **Parsing**: Run the OpenRTK data decoder executable inside the parser folder to parse the logged OpenRTK330L binary files

      .. code-block:: python

            cd C:\pythondriver-win\openrtk_data_parse
            .\openrtk_parse.exe -p ..\data\<OpenRTK data log folder>

      A few "csv" files are decocded from the "user_<time>.bin" and "debug_<time>.bin" output, the content of each of the "csv" files is described in its file header. 


      (Optional) On Windows 10, download `convbin.exe <https://virtualmachinesdiag817.blob.core.windows.net/tools/convbin.exe>`_ and run the program to decode the logged GNSS RTCM binary files to obtain `RINEX <https://www.igscb.org/wg/rinex/>`_ text files for quick checking.

  - Using the CAN

    User could use a CAN-USB (e.g. https://canable.io/) or CAN-TTL adapter to connect with the DB-9 male interface on the EVB to log and parse the CAN messages (click `here <https://openrtk.readthedocs.io/en/latest/communication_port/Can_port.html>`_ for definitions). Note that user has to write their own CAN message parsing code using the provided lib or open-source code from the adapter provider. 
