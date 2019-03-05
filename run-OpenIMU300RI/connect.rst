Connect OpenIMU300RI
====================

.. contents:: Contents
    :local:


.. note::
    * An external DC power supply will be needed.  The power supply must be able to produce 4.9V to 32V at 400mW
    * An RS232-to-USB adapter will be needed to connect the unit to the PC for RS232 communication.
    * A CAN bus adapter will be needed to connect the unit to the PC for CAN bus communication.

Refer to the images below:

*   **Connecting CAN/UART/Power**.  Align the keys on the unit and the cable connector.  Push the 6-pin cable connector into the unit connector.
*   **Double Locking the connector**: The connector is now locked to the unit.  However, an extra lock can be engaged by pushing the red latch under the black latch.
*   **Unlocking the connector**

    *   If engaged, pull the red latch away from the connector.
    *   Push down gently on the black latch at the top end of the connector (as shown on the image) and release.
    *   Pull the cable connector away from the unit.
*   **RS232 connection**.  Connect the RS232 connector to an RS232-to-USB adapter.  Connect the USB connector from the adapter to the Windows computer.
*   **CAN bus connection**.  Connect the RS232 connector to the CAN bus adapter adapter.  Connect the USB connector from the adapter to the Windows computer.
*   **JTAG debugger connection**.  Connect the keyed connector on the JTAG debugger cable to the keyed connector on the eval board.  Connect the USB cable to the JTAG debugger and the Windows computer.
*   **Power/Ground connection**.  With the power supply turned off, connect the power and ground leads to the external power supply.
*   **Power**.  **There is no on/off switch on the unit or the eval board attached to the unit**.  Turn the power supply on or off to supply power or remove power.  


+---------------------------------------------------------+-------------------------------------------------------------------------------+
| .. figure:: ../media/OpenIMU300RI-ConnectorCloseup.png  | .. figure:: ../media/OpenIMU300RI-EvalConfiguration.png                       |
|                                                         |                                                                               |
+---------------------------------------------------------+-------------------------------------------------------------------------------+
|    **OpenIMU300RI Connector**                           |    **OpenIMU300RI JTAG Debugger Connections**                                 |
+---------------------------------------------------------+-------------------------------------------------------------------------------+



