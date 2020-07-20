How to Use OpenRTK330 EVK?
============================

Note the usage of OpenRTK330 is described with the OpenRTK330 EVK. There are two types of user APP provided to interact with both the module and a NTRIP server over the internet providing GNSS correction data for RTK positioning:

* Android: the "OpenRTK" Android App with the following contents 
  
  * Bluetooth connectivity to module 
  * 4G connectivity to NTRIP server
  * Map display with user trajectory and positioning infromation
  * Module settings on positioning parameters and user configuration

    .. image:: /media/Mobile_APP.png

* PC: using the Ethernet interface

  * Ethernet connectivity between module and NTRIP server with a lightweight TCP/IP stack embedded in firmware
  * Module settings on positioning parameters and user configuration with a web GUI embedded in firmware
  * Map and positioning information display on the online web GUI (`"OpenRTK Monitor" <https://developers.aceinna.com/devices/rtk>`_) of Aceinna developer website

    .. image:: /media/PC_tool.png

The following two subsections cover the detailed steps of using the two types of user App.

.. toctree::
    :maxdepth: 1
    :hidden:

    useOpenRTK/On-a-PC
    useOpenRTK/Go-Mobile
    useOpenRTK/EVK-Vehicle-Installation
