******************************************************************
Vertical-Gyro / Attitude and Heading Reference System Application
******************************************************************

.. contents:: Contents
    :local:

.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


The Vertical-Gyro (VG) / Attitude and Heading Reference System (AHRS) application enables the
OpenIMU hardware to fuse inertial-sensor information (accelerometers, rate-sensors, and — for the
AHRS — magnetometers) to generate an attitude solution.  The solution makes use of the high
data-rate (DR) rate-sensor output to propagate the attitude forward in time while using the
accelerometers and magnetometers as references to correct for estimated rate-bias errors and
attitude-errors at a lower DR.

The mathematics behind the algorithm are quite a bit more complicated than the math associated with
the Static-Leveler application.  The full description is not discussed here, as .  However, the
complete formulation is provided in the "Ready-to-use Applications" section.

The VG/AHRS example application performs the following functions:

    * Sets the default OpenIMU configuration
    * Acquires sensor data  - acceleration, angular-rate, local magnetic-field, and sensor temperature data
    * Executes the VG/AHRS algorithm
    * Populates the output data structure 
    * Generates and sends the following output message to the UART - the output message description is To Be Provided
