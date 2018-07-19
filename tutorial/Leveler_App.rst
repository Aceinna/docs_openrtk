***************************
Static Leveler Application
***************************

.. contents:: Contents
    :local:


Overview
=========

The static-leveler app will enable the OpenIMU hardware to measure roll and pitch angles (the
angles that the x and y-axes are rotated away from level) using only accelerometer measurements.
This simple example will build on the IMU application and help you become acquainted with the 
following parts of the OpenIMU software platform:

    * Acquiring sensor data
    * Filtering data (to be added)
    * Obtaining sensor data within algorithm function
    * Using built-in math function
    * Creating an algorithm
    * Using the serial-debugger
    * Generating serial output messages
    * Setting the default OpenIMU configuration
    * Setting up and using the python-based message decoder
    * Using the Aceinna Navigation Studio to capture data


Just as for the IMU application, the application development is broken up into a series of sections
that build upon one another, as follows:

.. toctree::
    :maxdepth: 3

    tutorial/DataAcquisition
    tutorial/Requirements
    tutorial/LevelerAlgorithm
    tutorial/SerialDebugger
    tutorial/SerialMessaging
    tutorial/DefaultConfiguration
    tutorial/PythonMessageDecoder
    tutorial/NavigationStudio