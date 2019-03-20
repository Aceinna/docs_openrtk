*********************
Framework Application
*********************

.. contents:: Contents
    :local:


The Framework Application provides supporting functions to create an example application that provides all of the OpenIMU Core functionality.
All of the other example applications are based on the Framework application.  

While the exact combination of sensor data you use will depend upon the ultimate goal of your
project, at least a subset of this data is required to create an application that estimates the
attitude or position of the system.

The Platform application simply reads the accelerometers, the angular 
acceleration sensors, and the magnetometers and provides a simple 
output message with the time and the sensor values.

Every OpenIMU unit is loaded with the Platform application during production.
