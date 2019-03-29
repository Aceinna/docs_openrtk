***************************
Static-Leveler Application
***************************

.. contents:: Contents
    :local:

.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


The static-leveler application enables the OpenIMU hardware to provide roll and pitch estimates (the
angles that the x and y-axes are rotated away from level) using only accelerometer measurements.
This simple example is based on the *IMU Example Application*

The Static Leveler application performs the following functions:

    * Sets the default OpenIMU configuration for the Leveler application
    * Acquires Sensor Data - acceleration, angular-rate, local magnetic-field,
      and sensor temperature data
    * Executes the Leveler application algorithms and other relevant math
      functions to create output data:

        * Compute the acceleration unit-vector.
        * Normalize using the magnetometer readings.
        * Form the gravity vector in the body-frame.
        * Form the roll and pitch Euler angles from the gravity unit vector.

    * Generates a serial output message [#fn1]_ consisting of the following:

        * A relative time measurement (both integer and decimal values)
        * Acceleration readings in :math:`[g]`
        * Rate-sensor readings in [°/s]
        * Magnetic-field readings in :math:`[G]`
        * Sensor temperature readings in [°C]


.. rubric:: Footnotes

.. [#fn1] The output message is the same as for the IMU application, but tailored by the Leveler algorithm
