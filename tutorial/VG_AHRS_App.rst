******************************************************************
Vertical-Gyro / Attitude and Heading Reference System Application
******************************************************************

.. contents:: Contents
    :local:


The Vertical-Gyro (VG) / Attitude and Heading Reference System (AHRS) application enables the
OpenIMU hardware to fuse inerial-sensor data from accelerometers, rate-sensors, and magnetometers
to generate an attitude solution.  The solution makes use of the high data-rate of rate-sensors to
propagate the attitude forward in time while using the accelerometers and magnetometers as
references to correct for bias-errors in the rate-sensors and attitude-errors in the estimation.

The mathematics behind the algorithm is quite a bit more complicated than the math associated with
the `Static-Leveler <LevelerAlgorithm.html#static-leveler-algorithm>`__





take inertial-sensor data from accelerometers, rate-sensors, magnetometers, 
and/or GPS to form a fused solution.

This application is under development.  Please check back.




The static-leveler applicaton enables the OpenIMU hardware to provide roll and pitch estimates (the
angles that the x and y-axes are rotated away from level) using only accelerometer measurements.
This simple example builds on the
`IMU application <IMU_App.html#inertial-measurement-unit-application>`__ and helps you become
acquainted with the following parts of the OpenIMU software platform:

    * Acquiring sensor data
    * Obtaining sensor data within the algorithm function
    * Using built-in math functions
    * Creating an algorithm
    * Using the serial-debugger
    * Generating serial output messages
    * Setting the default OpenIMU configuration
    * Setting up and using the python-based message decoder
    * Using the Aceinna Navigation Studio to capture data


Just as for the `IMU application <IMU_App.html#inertial-measurement-unit-application>`__,
application development is broken up into a series of sections that build upon one another, as
follows:

.. toctree::
    :maxdepth: 1

    VG_AHRS/DataAcquisition
    VG_AHRS/Requirements
    VG_AHRS/LevelerAlgorithm
    VG_AHRS/SerialDebugger
    VG_AHRS/SerialMessaging
    VG_AHRS/DefaultConfiguration
    VG_AHRS/PythonMessageDecoder
    VG_AHRS/NavigationStudio

