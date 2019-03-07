Connect the OpenIMU300RI Components
===================================

.. contents:: Contents
    :local:


.. note::
    * An external DC power supply will be needed.  The power supply must be able to produce 4.9V to 32V at 400mW
    * An RS232-to-USB adapter will be needed to connect the unit to the PC for RS232 communication.
    * A CAN bus adapter will be needed to connect the unit to the PC for CAN bus communication.  A CAN adapter with a 9-pin D-sub connector will mate directly with the kit's CAN 9-pin D-sub connector

Refer to the images below:

*   **Connecting Main connector (CAN/UART/Power)**.  Align the keys on the unit and the cable connector.  Push the 6-pin cable connector into the unit connector.
*   **Double Locking the connector**: The connector is now locked to the unit.  However, an extra lock can be engaged by pushing the red latch under the black latch.  This prevents the disengagement button from being depressed.
*   **Unlocking the connector**

    *   If engaged, pull the red latch away from the connector toward the cable.
    *   Push down on the black disengagement button in the middle of the connector.
    *   Pull the cable connector away from the unit.

*   **RS232 connection**.  Connect the RS232 9-pin D-sub connector to a PC's RS232 9-pin D-sub connector or through an RS232-to-USB adapter. 

*   **CAN connection**.  The CAN bus connector is used to connect units to the CAN bus.

*   **JTAG debugger connection**.  

    *   Connect the keyed connector on the ribbon cable to the keyed connector on the eval board.
    *   Connect the keyed connector on the the other end of the ribbon cable to the JTAG debugger.  
    *   Connect the USB cable to the JTAG debugger and the host computer.

*   **Power/Ground connection**.  With the power supply turned off, connect the power and ground leads to the external power supply.
*   **Power**.  **There is no on/off switch on the unit or the eval board attached to the unit**.  Turn the power supply on or off to supply power or remove power.  


+---------------------------------------------------------+-------------------------------------------------------------------------------+
| .. figure:: ../media/OpenIMU300RI-ConnectorCloseup.png  | .. figure:: ../media/OpenIMU300RI-EvalKit.png                                 |
|                                                         |                                                                               |
+---------------------------------------------------------+-------------------------------------------------------------------------------+
|    **OpenIMU300RI Connector**                           |    **OpenIMU300RI JTAG Debugger Connections**                                 |
+---------------------------------------------------------+-------------------------------------------------------------------------------+



