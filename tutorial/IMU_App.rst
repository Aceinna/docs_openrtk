***************************************
Inertial Measurement Unit Application
***************************************

.. contents:: Contents
    :local:


The Inertial Measurement Unit (IMU) application enables the OpenIMU hardware to provide
inertial-sensor data from accelerometers, rate-sensors, and magnetometers.  While the exact
combination of sensor data depends upon the ultimate goal of your project, at least a subset of
this data is required to create applications that estimate the attitude or position of systems.
This example forms the basis of future application development and helps you become acquainted with
the following parts of the OpenIMU software platform:

    * Acquiring sensor data
    * Filtering data (using built-in low-pass filters)
    * Debugging with the serial-debugger
    * Generating serial output messages
    * Setting the default OpenIMU configuration
    * Setting up and using the python-based message decoder
    * Using the Aceinna Navigation Studio to capture data


Application development is broken up into a series of sections
that build upon one another, as follows:

.. toctree::
    :maxdepth: 1

    IMU/DataAcquisition
    IMU/Requirements
    IMU/SerialDebugger
    IMU/SerialMessaging
    IMU/DefaultConfiguration
    IMU/PythonMessageDecoder
    IMU/NavigationStudio
    
    