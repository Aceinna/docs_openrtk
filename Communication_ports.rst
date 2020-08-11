Communication Ports and Operation
---------------------------------

USER UART has serial port IAP (program firmware APP) function, ST UART PROG serial port is 
the serial port for SDK firmware programming, users must connect. BT UART and ETH can pull 
base rtcm3 for RTK operation, users need to choose at least one connection. Other interface 
users can connect according to their needs. The hardware design can refer to OpenRTK330 EVK.


.. toctree::
    :maxdepth: 1
    :hidden:

    ports/User_port
    ports/ST_GNSS_UART1

