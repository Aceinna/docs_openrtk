********************
Serial Debugging
********************

.. contents:: Contents
    :local:


Generating Debug Messages
==========================

**Creating the Message**:

Debug messages, using the built-in debugging capability of the OpenIMU platform, are added to the
IMU application to verify that the firmware obtains the correct sensor reading; the complete
implementation is found in *dataProcessingAndPresentation.c* in the IMU application code.  The
relevant debugger calls are:

::

    DebugPrintFloat("Time: ", 0.001 * gIMU.timerCntr, 3);
    DebugPrintFloat(", AccelZ: ", gIMU.accel_g[Z_AXIS], 3);
    DebugPrintFloat(", RateZ: ", gIMU.rate_degPerSec[Z_AXIS], 3);
    DebugPrintFloat(", MagX: ", gIMU.mag_G[X_AXIS], 3);
    DebugPrintFloat(", Temp: ", gIMU.temp_C,2);
    DebugPrintEndline();


In the output message, z-axis acceleration and rate-sensor measurements, provided in :math:`[g]`
and :math:`[°/s]`, are obtained along with x-axis magnetic-field readings (in :math:`[G]`) and
board-temperature (in :math:`[°C]`).  This subset of sensor information is selected to test the
output of all sensors, while keeping the size of the debug message small.


Arguments to *DebugPrintFloat()* consist of:

    1. A character-string describing the output message
    2. The floating-point value to be output
    3. The number of significant digits in the output message


In this example, only *DebugPrintFloat()* is used to output a debug message, other debug message
functions are available. In particular, the following messages (provided in *debug.c*) form the
complete list:

::

    DebugPrintString();
    DebugPrintInt();
    DebugPrintLongInt();
    DebugPrintHex();
    DebugPrintFloat();
    DebugPrintEndline();


Compile and Test
=================

The final step is to build and upload the firmware to the OpenIMU hardware using the PIO framework.
When complete, use a terminal program (such as TeraTerm in Windows) to connect to the appropriate
COM port to assess if the program is operating as expected.


**Debug Communication Settings**:

Debug messages are provided as serial messages over the third port of the OpenIMU platform. When
connected to a PC, the device generates four COM ports.  In this case, the ports are 40, 41, 42,
and 43. The first COM port is the serial messaging port (discussed in XXX), the second port can
be used for serial inputs to the platform (such as GPS), and the fourth is unconnected.


The nominal serial baud-rate setting is 38.4 kbps. This can be set to other rates, such as 57.6
kbps, 115.2 kbps, or 230.4 kbps via the argument to *InitDebugSerialCommunication()*, found in
*main.c*.  For the IMU application, this value was changed to 230.4 kbps.


**System Testing using Debug Communications**:

To test the OpenIMU output, perform the following:

    1. Place the unit on a level table top
    2. With the unit sitting flat, the z-axis acceleration will be close to -1.0 :math:`[g]`
    3. Rotate the unit clockwise (about the positive z-axis) to generate a positive z-axis
       angular-rate
    4. Orient the unit so the y-axis is aligned with magnetic-north.  This results in an x-axis
       magnetic-filed reading close to zero :math:`[G]`.  Orienting the unit's x-axis in any other
       compass direction will result in a non-zero magnetic-field reading that increases until the
       axis is pointed along the north/south direction, at which it reaches its maximum value.
    5. Temperature readings reflect values slightly higher than the ambient temperature, as the
       readings reflect the temperature of the electronics.

The results of these statements are found in the following figure:

.. _fig-term-imu-debug-out:

.. figure:: ./media/IMU_DebugCapture.PNG
    :alt: TerminalDebugOutput
    :width: 6.0in
    :align: center

    **Figure 1: IMU Debug Output**

This output provides confidence that the IMU is obtaining the correct sensor measurements.


**Suggested Operation**

During normal operations, when using the OpenIMU in your system, it is best to disable the debug
output.  This will reduce the load on the platform and free up the processing capability for other
tasks.

