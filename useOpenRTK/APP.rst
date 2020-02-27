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
1. Power on the OpenRTK330 EVB using a 9-12 v DC adaptor or a microUSB cable connected to a PC (desktop, Raspberry Pi etc.), and check the YELLOW, RED AND GREEN LED lights to confirm valid firmware

  - YELLOW: flashing light indicating GNSS chipsets is powered on with 1PPS signal output correctly 
  - GREEN: flashing light indicating OpenRTK330 RTK or INS App is running correctly 

2. **Connection**

 - Enable "Bluetooth" on your Anroid device
 - Open the OpenRTK Andorid App and enable "Location" access for "OpenRTK" App on your Anroid device 
 - As shown by the picture below, go to the "Connect" tab and click the "search" icon to search for your device. If your OpenRTK330 device is found, a Bluetooth device ID appears on the "Connect" list. By factory setting, the Bluetooth device ID is "OpenRTK_<four digits>" and the four digits are the last four digits of your OpenRTK330 module S/N.

  .. image:: ../media/connect.jpg
    :align: center
    :scale: 18%   

 - After it’s connected, you can see the following interface.

  .. image:: ../media/connectLog.jpg
    :align: center
    :scale: 18%   

3. **Network Configuration**

  - *RTK Type*: 

     - RTK: get NEMA(GPGGA) from device,get RTCM from Aceinna server. 
     - cloudRTK: get RTCM from device, get NEMA(GPGGA) from Aceinna server. 

   For more details, please refer to `RTK/Cloud RTK <https://openrtk.readthedocs.io/en/latest/Network/rtk_cloudrtk.html>`__.
  - *Use Local Service*:

     - ON: you can use other service, and you need input its URL and Port.
     - OFF: use Aceinna OpenRTK service.

    .. image:: ../media/networkConfig.jpg
       :align: center
       :scale: 18%
      

 **2. User Configuration**.

  You can swipe left or click the icon "≡" for more user configuration, as flowing picture.

  .. image:: ../media/leftMenu.png
       :align: center
       :scale: 18%

  - *Cloud RTK*: show API key info or generate key.

  .. image:: ../media/CloudRTK.png
         :align: center
         :scale: 18%

  - *Device Setting*: change device setting, like Bluetooth name, baud rate, output packet and so on.
    
    .. image:: ../media/customDeviceConfig.jpg
         :align: center
         :scale: 18%

  - *Debug Mode*: Costum some map settings
    
    .. image:: ../media/mapConfig.jpg
         :align: center
         :scale: 18%

  - *Save result*: Save result information in GPGGA format if switch is on. The storage path is *Android/data/com.aceinna.rtk/files/log*.

 
Map
~~~

 - Livemap

  .. image:: ../media/offlineMap.jpg
    :align: center
    :scale: 18%   

 - Track map

  .. image:: ../media/trajectory.jpg
    :align: center
    :scale: 18%   


Aceinna Network service subscription
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 **Sign up**. you need a account to login in. The account is same as
 `Aceinna Developer Site <https://developers.aceinna.com/>`__ and
 `Aceinna Fourm <https://forum.aceinna.com//>`__. You can sign up an account,
 or log in quickly using your github account.

 .. image:: ../media/login.jpg
    :align: center
    :scale: 18%   

 **Generate API**. If you do not have the API key, you need generate API
 Key to use Aceinna RTK network.
  1. click next.
   .. image:: ../media/generate-step-1.jpg
     :align: center
     :scale: 18%

  2. set a number, it means how many devices you can use this account at the same time.
   .. image:: ../media/generate-step-2.jpg
     :align: center
     :scale: 18%

  3. click finish to generate API key.
   .. image:: ../media/generate-step-3.jpg
     :align: center
     :scale: 18%

    
