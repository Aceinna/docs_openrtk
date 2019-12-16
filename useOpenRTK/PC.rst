PC
===

OpenRTK acts as NTRIP client connects with NTRIP server to fetch
RTCM/NMEA data from ethernet in RTK, and then send to Aceinna server,
after the calculation of the server, the data is returned, and then
written to the device. The RTK device will calibrate according to the
data.

.. image:: ../media/PC_tool.png

Installation and initialization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 ::

 1. Plug in a RJ45 cable from the host(e.g. Router or Desktop) to the
    ethernet port of OpenRTK.
 ::

 2. DHCP IP address is used by OpenRTK as default, if no success,
    manually setup a STATIC IP (ip = 192.168.1.110, netmask =
    255.255.255.0, gateway = 192.168.1.1).
 ::

 3. Visit the embedded web site ("http://openrtk") on the same LAN as
    OpenRTK.

Account Registration
~~~~~~~~~~~~~~~~~~~~

 **Sign up**. You need a account to login in. The account is same as
 Aceinna Developer Site and Aceinna Fourm. You can click 'Join us' to sign up your account.

 .. image:: ../media/login_PC.png
     :align: center
     :scale: 50%

Aceinna Network service subscription
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 **Generate API**. If you do not have the API key, you need `generate API
 key <https://openrtk.readthedocs.io/en/latest/Network/getapikey.html>`__  
 to use Aceinna RTK network and set the number of allowed devices.

 .. image:: ../media/signup.png
    :align: center
    :scale: 50%

Configuration
~~~~~~~~~~~~~

 **1. Ntrip Configuration**.

  - *RTK Ntrip*: If you choose Local Service, ntrip client will use
    username and apikey in login. Otherwise, ntrip client will use
    username and password in ntripcfg.
  - *RTK Type*: 

        - LocalRTK: get NEMA(GPGGA) from device, get RTCM from Aceinna server. 
        - cloudRTK: get RTCM from device, get NEMA(GPGGA) from Aceinna server.
  - *Ntrip state*:

        - ON: etnernet ntrip on, forbid bluetooth rtcm from mobile.
        - OFF: ethernet ntrip off, allow bluetooth rtcm from mobile.
          state can be controlled by 'START' button.

  .. image:: ../media/ntripcfg.png
     :align: center
     :scale: 50%

 **2. User Configuration**.

    - change device setting, like baud rate, output packet and so on.

    .. image:: ../media/usercfg.png
       :align: center
       :scale: 50%

 **3. Ethernet Configuration**.

    -  *IP mode*: static ip or dhcp ip,if dhcp fail, it will use static ip.
    -  *Static ip*: include ip, netmask and gateway.
    -  *RESTART*: if dhcp over and fail, you can click "RESTART" to restart
       DHCP.

    .. image:: ../media/ethcfg.png
       :align: center
       :scale: 50%

Connection
~~~~~~~~~~

 - You should download python driver from https://github.com/Aceinna/python-openimu/tree/ans-devices. 
   Then, you need to run main.py (python main.py -p 9753).

 - Device information is exposed on the DEVICE INFO page (https://developers.aceinna.com/devices/rtk). 
   The default webserver port for rtk is 9753. If you change the port on the web, you should also 
   change the port when running main.py.

 - Connection Status is shown on the link symbol at the top right hand side of the page.
 
 .. image:: ../media/connect.png
   :align: center
   
 - You can visit https://developers.aceinna.com/devices/files to see      RTK LOGIN LOGS.

    .. image:: ../media/serverrtk.png
        :align: center 

Map
~~~

 Play button will be enabled when openrtk is connected. Click 'play' to see the openrtk output information.

 .. image:: ../media/map.png
   :align: center
   :scale: 50%

 .. image:: ../media/skyview.png
   :align: center
   :scale: 50%

