********************
Serial Debugging
********************

.. contents:: Contents
    :local:


Steps to Generating Debug Messages
===================================

Debugging messages, using the built-in debugging capability of the OpenIMU platform, are added to
the leveler algorithm to determine if the algorithm is generating the correct results. Just as for
the algorithm, the complete implementation is found in UserAlgorithm.c. However, the relevant
debugger calls are below:

::

    DebugPrintFloat("Time: ", gLeveler.timerCntr * 0.001, 3);
    DebugPrintFloat(", Roll: ", gLeveler.measuredEulerAngles_BinN[ROLL] * RAD_TO_DEG, 5);
    DebugPrintFloat(", Pitch: ", gLeveler.measuredEulerAngles_BinN[PITCH] * RAD_TO_DEG, 5);
    DebugPrintEndline();
    
    
In the output message, time (generated from the algorithm counter) is provided along with the
computed roll and pitch angles, converted from [radians] to [degrees], at a 2 [Hz] output rate.
The results of these statements are found in the following figure:

.. _fig-term-debug-out:

.. figure:: ./media/Leveler_DebugCapture.PNG
    :alt: TerminalDebugOutput
    :width: 5.0in
    :align: center

    **Figure 1: Leveler Algorithm Debug Output**

This statements provide confidence that the algorithm is generating the correct roll angle of
approximately 15 [degrees] based upon the positioning of the test unit.


Debug messages are provided as serial messages over the third port of the OpenIMU platform. When
connected to a PC, the device generates four COM ports.  In this case, the ports are 31, 32, 33,
and 34. The first COM port is the serial messaging port (to be discussed shortly), the second can
be used for serial inputs to the platform, and the fourth is unconnected. (REFER TO SECTION BLAH).


The nominal serial baud-rate setting is 38.4 kbps. This can be set to other rates, such as 57.6
kbps or 115.2 kbps via *InitDebugSerialCommunication()*, found in main.c.


In this example, only *DebugPrintFloat()* was used to output a debug message, other debug message
functions are available. In particular, the following messages (provided in debug.c) form the
complete list:

::

    DebugPrintString();
    DebugPrintInt();
    DebugPrintLongInt();
    DebugPrintHex();
    DebugPrintFloat();
    DebugPrintEndline();


The arguments to DebugPrintFloat() are:

    1. Character string describing the message
    2. The float-point value to be output
    3. The number of significant digits in the output message


Compile and Test
=================

The final step is to build and upload the firmware to the OpenIMU hardware using the PIO frameword.
When complete, use a terminal program (such as TeraTerm in Windows) to connect to the appropriate
COM port and see if the program is operating as expected.

