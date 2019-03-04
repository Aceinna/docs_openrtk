OpenIMU300RI Evaluation Kit Setup 
=================================

.. contents:: Contents
    :local:

To set up OpenIMU300RI evaluation kit for development you'll need to perform next steps:

 1. Unpack OpenIMU300RI evaluation kit.
 2. **Do not connect power supply to the unit yet.**
 3. Connect OpenIMU300RI evaluation board to a PC via the provided connector. The connector provides connections for power/ground, the CAN bus, and the UART.
 4. Connect ST-Link debugger to the PC via USB cable. 
 5. Connect OpenIMU300RI evaluation board to ST-Link debugger using provided 20-pin flat cable.
 6. Set the power supply to DC 12V.
 7. Connect the power leads from the Eval Kit to the power supply.
 8. Turn power supply on.

Now you are ready to install OpenIMU's open source tool chain to develop, debug and test your application.

.. toctree::
    :maxdepth: 2

    run-OpenIMU300RI/connect
    run-OpenIMU300RI/jtag-download
    run-OpenIMU300RI/debug
    run-OpenIMU300RI/graphing



.. figure:: media/OpenIMY300RI-EvalConfiguration.png
    :width: 5.0in
    :height: 5.0in
    :align: left

    **OpenIMU300RI Evaluation Kit**

