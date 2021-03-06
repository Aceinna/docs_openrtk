OpenRTK Developer Manual
========================

.. image:: media/OpenRTK.jpg
   :align: center
   :width: 5.0in
   :height: 5.7in

OpenRTK is an integrated GNSS (Global Navigation Satellite System) high precision 
chip and precisely calibrated Inertial Measurement Unit open-source platform for 
the development of navigation and localization algorithms. Users are able to quickly 
develop and deploy custom navigation/localization algorithms and custom sensor integrations 
on top of the OpenRTK platform.  OpenRTK also has pre-built drivers in Python as well as 
a developer website - Aceinna Navigation Studio (ANS). These tools make logging and plotting data, 
including custom data structures and packets very simple.

**Social:** `Twitter <https://twitter.com/MEMSsensortech>`_ |
`Medium <https://medium.com/@mikehorton>`_

.. raw:: latex

   \part{About OpenRTK}

.. toctree::
    :caption: About OpenRTK
    :maxdepth: 1
    :hidden:
    :titlesonly:

    intro

.. raw:: latex

   \part{Tutorial}

.. toctree::
    :caption: Tutorial
    :maxdepth: 1
    :hidden:
    :titlesonly:

    quick_start.rst
    useOpenRTK
    firmware_upgrade.rst
    openarc_service.rst
..  build_firmware.rst

.. raw:: latex

   \part{RTK/IMU Modules}

.. toctree::
    :caption: The OpenRTK330LI Module
    :maxdepth: 1
    :hidden:
    :titlesonly:

    Module-OpenRTK330LI

.. raw:: latex

   \part{Evaluation Kits}

.. toctree::
    :caption: The OpenRTK330LI EVK
    :maxdepth: 1
    :hidden:

    EVK-OpenRTK330LI

.. raw:: latex

   \part{Communication protocol}

.. toctree::
    :caption: Interface and Protocol
    :maxdepth: 2
    :hidden:

    communication_port/User_uart
    communication_port/Debug_uart
    communication_port/Can_port
    communication_port/nmea

.. raw:: latex

   \part{RTKlib tools}

.. toctree::
    :caption: RTKLIB Tools for OpenRTK
    :maxdepth: 3
    :hidden:

    rtklib_tool/overview
    rtklib_tool/strsvr_user

