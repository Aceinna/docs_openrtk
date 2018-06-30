Building Your First Custom App
===============================

.. contents:: Contents
    :local:
    

To get you acquainted with the environment, we will walk you through the development of a simple
application: a static-leveler app.  A static-leveler uses accelerometer readings to measure the
local gravity-field and compute the two-axis attitude (roll and pitch angles) of a body relative to
the local-level frame.
 
This app will enable the OpenIMU hardware to measure roll and pitch angles (the angles that the x
and y-axes are rotated away from level) using only accelerometer measurements.  This simple
examples will help you become acquainted with the following parts of the OpenIMU software platform:

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


The algorithm development description is broken up into a series of sections that build upon one
another, as follows:

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
    