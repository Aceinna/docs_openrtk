OpenIMU300RI Evaluation Kit Setup
=================================

.. contents:: Contents
    :local:

The following pages provide the steps needed to set up and use the OpenIMU300RI Evaluation Kit:

*   Connect the OpenIMU300RI Components
*   The following activities are addressed in the "Development Tools »
    Debugging using the PlatformIO Debugger and the JTAG Debug Adapter" section

    *   Download App with JTAG
    *   Debug with PlatformIO Debugger and JTAG Debug Adapter
    *   Graphing & Logging IMU Data using the Acienna Navigation Studio

.. note::
    The RS232/CAN/Power cable shown in the image is similar to the cable that will be supplied with the kit.  It is for information only.

.. figure:: media/OpenIMU300RI-EvalKit.png
    :height: 684

    **OpenIMU300RI Evaluation Kit**

**Connect the OpenIMU300RI Evaluation Kit Components**

.. note::
    *   An external DC power supply will be needed.  The power supply must be able to produce 4.9V to 32V at 400mW
    *   An RS232-to-USB adapter will be needed to connect the unit to the PC for RS232 communication.
    *   The CAN interface is available to connect with other CAN network nodes.
    *   If CAN bus communication monitoring by a PC is desired, a CAN interface adapter
        will be needed to connect the the PC to the CAN network for CAN bus communication.

The following directions for connecting the unit to a PC and to the CAN network refer to the images below:

*   **Connecting Main connector (CAN/UART/Power)**.  Align the keys on the unit and the cable connector.
    Push the 6-pin cable connector into the unit connector.
*   **Double Locking the connector**: The connector is now locked to the unit.  However, an extra lock
    can be engaged by pushing the red latch under the black latch.  This prevents the disengagement button from being depressed.
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


+------------------------------------------------------+---------------------------------------------+
| .. figure:: media/OpenIMU300RI-ConnectorCloseup.png  | .. figure:: media/OpenIMU300RI-EvalKit.png  |
+------------------------------------------------------+---------------------------------------------+
| **OpenIMU300RI Connector**                           | **OpenIMU300RI JTAG Debugger Connections**  |
+------------------------------------------------------+---------------------------------------------+
