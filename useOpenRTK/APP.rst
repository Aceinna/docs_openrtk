Cloud RTK APP
=======================

OpenRTK Tool is work for Aceinna OpenRTK app. APP get RTCM/NMEA data
from bluetooth in RTK, and then send to Aceinna server,after the
calculation of the server, the data is returned, and then written to the
device. The RTK device will calibrate according to the data.

Installation
---------------

::

    You can scan the QR code to download, currently only the android version and make sure you Android version is 8.0 or above. after downloaded open the apk file to install. And please make the app in white list.
.. image:: ../media/ercode.png
   :align: center
   :scale: 50%

How to Use
----------

1. Sign up. you need a account to login in.The account is same as
   `Aceinna Developer Site <https://developers.aceinna.com/>`__ and
   `Aceinna Fourm <https://forum.aceinna.com//>`__. You can apply one,
   or log in quickly using your github account.
 .. image:: ../media/login.jpg
   :align: center
   :scale: 15%   

2. Generate API. If you do not have the API key, you need generate API
   Key.
 -  step 1, click next.
 .. image:: ../media/generate-step-1.jpg
   :align: center
   :scale: 15%

 -  step 2, enter a number, it means how many devices you can use this account use at same time.
 .. image:: ../media/generate-step-2.jpg
   :align: center
   :scale: 15%

 -  step 3, click finish to generate APIkey.
 .. image:: ../media/generate-step-3.jpg
   :align: center
   :scale: 15%

3. APP screenshots
 .. image:: ../media/info2_meitu_1.jpg
   :align: center
   :scale: 15%

 .. image:: ../media/info1.jpg
   :align: center
   :scale: 15%

 .. image:: ../media/logs.jpg
   :align: center
   :scale: 15%

 .. image:: ../media/map.jpg
   :align: center
   :scale: 15%

 .. image:: ../media/offlinemap.jpg
   :align: center
   :scale: 15%

 .. image:: ../media/livemap.jpg
   :align: center
   :scale: 15%

 .. image:: ../media/networkSetting.jpeg
   :align: center
   :scale: 15%
 1.  you can see your api key or generate key.
 2.  can change device setting, like change bluetooth name, change baud
     rate and so on, you can do that only when the rtk device is
     connected.
 3.  Save result information in GPGGA format if switch is on. The storage
     path is ``Android/data/com.aceinna.rtk/files/log``.
 4.  Custom some map setting, like line points, skip points to show and
     so on...
 5.  Logout
 6.  Show bluetooth device list
 7.  Show log when device is connect
 8.  According the NMEA data draw points in map
 9.  change setting. you can connect your local server to analyze NMEA
     data or RTCM data.
 10. search RTK device with bluetooth
 11. view the offline data, you can select a NMEA data file (GPGGA).
 12. Only when the device is connected, you can draw a live map.
 13. It only use when the device type is RTK. It will send data to server
     if switch is on.
 14. Follow/not follow current point.
 15. make sure which type your device support.
  -  RTK: get NEMA(GPGGA) from device,get RTCM from Aceinna server.
  -  cloudRTK: get RTCM from device, get NEMA(GPGGA) from Aceinna server.
 16. you can use your local service to process data what from RTK device.