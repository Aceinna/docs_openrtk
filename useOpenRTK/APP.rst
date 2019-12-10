Mobile
======

OpenRTK works for Aceinna OpenRTK app. APP get RTCM/NMEA data
via Bluetooth from RTK device, and then send to Aceinna server on Internet,
calculation results to be sent to the device. The RTK device will calibrate according to the data.

.. image:: ../media/Mobile_APP.png

Installation
------------


You can scan the QR code to download, currently only the android version and make sure your Android 
version is 8.0 or above. After downloaded open the apk file to install. Please make the app in white list.

.. image:: ../media/ercode.png
   :align: center
   :scale: 70%


How to Use
----------

1. Sign up. you need a account to login in.The account is same as
   `Aceinna Developer Site <https://developers.aceinna.com/>`__ and
   `Aceinna Fourm <https://forum.aceinna.com//>`__. You can apply one,
   or log in quickly using your github account.
 .. image:: ../media/login.jpg
   :align: center
   :scale: 20%   

2. Generate API. If you do not have the API key, you need generate API
   Key.
 -  click next.
 .. image:: ../media/generate-step-1.jpg
   :align: center
   :scale: 20%

 -  enter a number, it means how many devices you can use this account at the same time.
 .. image:: ../media/generate-step-2.jpg
   :align: center
   :scale: 20%

 -  click finish to generate APIkey.
 .. image:: ../media/generate-step-3.jpg
   :align: center
   :scale: 20%

3. APP screenshots
 .. image:: ../media/info.jpg
   :align: left
   :scale: 18%

 .. image:: ../media/deviceList.jpeg
   :align: left
   :scale: 18%

 .. image:: ../media/logs.jpg
   :align: left
   :scale: 18%

 .. image:: ../media/offlineMap.jpg
   :align: left
   :scale: 18%

 .. image:: ../media/livemap.jpg
   :align: left
   :scale: 18%

 .. image:: ../media/networkSetting.jpeg
   :align: left
   :scale: 18%
 

 1.  Save result information in GPGGA format if switch is on. The storage path is `Android/data/com.aceinna.rtk/files/log`.
 2.  It only used when the device type is RTK. It will send data to server if switch is on.
 3.  Make sure which type your device support.
   - RTK: get NEMA(GPGGA) from device,get RTCM from Aceinna server. 
   - cloudRTK: get RTCM from device, get NEMA(GPGGA) from Aceinna server. 
 4.  you can use your local service to process data what from RTK device.
 5.  View the offline points, you can only use a NMEA data file (GPGGA)(`example <https://navview.blob.core.windows.net/web-resources/exampleFile/example.log>`__).