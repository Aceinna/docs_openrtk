*****************************************************
GPS / Inertial Measurement Unit (GPS/INS) Application
*****************************************************

.. contents:: Contents
    :local:

.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


The Inertial Measurement Unit (IMU) application enables the OpenIMU hardware to provide
inertial-sensor data from accelerometers, rate-sensors, and magnetometers.  This example forms the
basis of future application development for systems that use GPS data for Inertial Navigation.

The exact combination of sensor data you use will depend upon the ultimate goal of your
project.  However, at least a subset of this data is required to create an application 
that estimates position and heading.

The IMU application performs the following functions:

    * Set the default OpenIMU configuration for the IMU application
    * Acquire Sensor Data - acceleration, angular-rate, local magnetic-field, GPS data, and sensor
	  temperature data
    * If not already done, set up the python-based message decoder
    * Use the Aceinna Navigation Studio to capture data
    * Generate a serial output message consisting of the following:
    
        * A relative time measurement (both integer and decimal values)
        * Acceleration readings in :math:`[g]`
        * Rate-sensor readings in [°/s]
        * Magnetic-field readings in :math:`[G]`
        * Sensor temperature readings in [°C]

