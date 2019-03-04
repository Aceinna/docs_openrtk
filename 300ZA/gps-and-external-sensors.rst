GPS & External Sensors Interfaces
=================================

.. contents:: Contents
    :local:


.. note:: 

    The processes for installing GPS and other external sensors and their antennas are beyond the scope of this document.

.. note::

    The CAN Bus OpenIMU300xI units (RI and AI) do not provide signals for external sensor inputs or output, so this page is not relevant for those units

Connector inputs and outputs
============================

.. figure:: ../media/image2.png
    :height: 2.0in

    OpenIMU300xZ (ZA and RZ) Connector


**Table - Interface Connector External Sensors Pin Definitions**

+---------+-------------------------------+
| **Pin** | **Pin Description             |
|         | (OpenIMU300ZA)**              |
+---------+-------------------------------+
| 2       || Synchronization Input        |
|         |   1PPS Input for External GPS |
+---------+-------------------------------+
| 17      | External GPS UART TX          |
+---------+-------------------------------+
| 19      | External GPS UART RX          |
+---------+-------------------------------+

GPS related messages
====================

UART messages
-------------

**Table - GPS-Related UART Messages**

+---------------------+-----------------------------+
| **Message**         | **Field**                   |
+---------------------+-----------------------------+
| N0 (Output to user) | GPS Altitude [-100,16284] m |
|                     | GPS Latitude (r)            |
|                     | GPS Longitude (r)           |
+---------------------+-----------------------------+
| N1 (Output to user) | ITOW (sync to GPS)          |
+---------------------+-----------------------------+
| RF or WF            | User behavior switches -    |
| (Input from User)   | Use GPS                     |
+---------------------+-----------------------------+

SPI messages
------------

**Table - GPS-Related SPI Messages**

+---------------------------+-------------------------+
| **Message**               | **Field**               |
+---------------------------+-------------------------+
| N0_Burst (Output to user) | Same data as UART N0    |
+---------------------------+-------------------------+
| RF or WF                  | Same data as UART RF/WF |
| (Input from User)         | Use GPS                 |
+---------------------------+-------------------------+
