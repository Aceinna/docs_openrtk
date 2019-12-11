How to use OpenRTK?
===================

The previous section presented the OpenRTK330 EVK setup process. Once the device is ready and loaded with OpenRTK330 firmware, it requires internet connectivity to perform GNSS RTK positioning. The OpenRTK330 module acts as a NTRIP client and requests and fetches GNSS data corrections from a NTRIP server over the internet. This connectivity requires the OpenRTK module sending out its current position via NMEA GGA every one second to the NTRIP server over the internet.

The following pages cover:

*   Mobile
*   PC

.. toctree::
    :maxdepth: 1
    :hidden:

    useOpenRTK/mobile
    useOpenRTK/PC
