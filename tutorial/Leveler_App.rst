***************************
Static-Leveler Application
***************************

.. contents:: Contents
    :local:
    
.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


The static-leveler application enables the OpenIMU hardware to provide roll and pitch estimates (the
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
    * Using the `Aceinna Navigation Studio <https://developers.aceinna.com>`__ to capture data


Just as for the `IMU application <IMU_App.html#inertial-measurement-unit-application>`__,
application development is broken up into a series of sections that build upon one another, as
follows:

.. toctree::
    :maxdepth: 1

    Leveler/DataAcquisition
    Leveler/Requirements
    Leveler/LevelerAlgorithm
    Leveler/SerialDebugger
    Leveler/SerialMessaging
    Leveler/PythonMessageDecoder
    Leveler/NavigationStudio


A pre-built version of the `Leveler Application <https://developers.aceinna.com/apps>`__ can be
downloaded directly onto the OpenIMU hardware at the
`Aceinna Navigation Studio <https://developers.aceinna.com>`__ website.  The process mirrors the
once described for the `IMU application <IMU_App.html#inertial-measurement-unit-application>`__:

.. toctree::
    :maxdepth: 1

    IMU/Bootloader


