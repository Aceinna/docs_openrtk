*****************************************************
Inertial Measurement Unit (IMU) Application
*****************************************************

.. contents:: Contents
    :local:

.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


The Inertial Measurement Unit (IMU) application enables the OpenIMU hardware to provide
inertial-sensor data from accelerometers, rate-sensors, and magnetometers.

The exact combination of sensor data you use will depend upon the ultimate goal of your
project.  However, at least a subset of this data is required to create an application
that estimates attitude, position, and/or heading.

The IMU application performs the following functions:

    * Sets the default OpenIMU configuration for the IMU application
    * Acquires Sensor Data - acceleration, angular-rate, local magnetic-field, and sensor temperature data
    * Generates and sends the following output message to the UART:

        * A relative time measurement (both integer and decimal values)
        * Acceleration readings in :math:`[g]`
        * Rate-sensor readings in [°/s]
        * Magnetic-field readings in :math:`[G]`
        * Sensor temperature readings in [°C]
