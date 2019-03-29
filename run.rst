OpenIMU300ZA Evaluation Kit Setup
=================================

.. contents:: Contents
    :local:

The following pages provide the steps needed to set up and use the OpenIMU300ZA Evaluation Kit:

*   Connect the OpenIMU300ZA Components
*   The following activities are addressed in the "Development Tools »
    Debugging using the PlatformIO Debugger and the JTAG Debug Adapter" section

    *   Download App with JTAG
    *   Debug with PlatformIO Debugger and JTAG Debug Adapter
    *   Graphing & Logging IMU Data using the Acienna Navigation Studio

.. figure:: media/EvalKit.png
    :width: 6.0in
    :height: 6.0in
    :align: left

    **OpenIMU300ZA Evaluation Kit**

To set up OpenIMU300ZA evaluation kit for development you'll need to perform next steps:

 1. Unpack OpenIMU300ZA evaluation kit.
 2. Push power switch to "OFF" position.
 3. Connect OpenIMU300ZA evaluation board to the PC via USB cable. USB connection provides power to the test setup as well as connectivity between PC and IMU serial ports.
 4. Connect ST-Link debugger to the PC via USB cable.
 5. Connect OpenIMU300ZA evaluation board to ST-Link debugger using provided 20-pin flat cable.
 6. Push power switch to "ON" position.

Now you are ready to install OpenIMU's open source tool chain to develop, debug and test your application.
