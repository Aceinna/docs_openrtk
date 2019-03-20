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
complete formulation is provided in the "Ready-to-use Applications" section.  Just as for the IMU and 
Static-Leveler applications, the
development of the VG/AHRS application is broken up into a series of steps:

    * Setting the default OpenIMU configuration
    * Acquiring sensor data
    * Executing the VG/AHRS algorithm
    
          * Obtaining sensor data within the algorithm function
          * Executing the algorithm
          * Populating the output data structure within the algorithm function
          
    * Using the serial-debugger
    * Generating serial output messages
    * Setting up and using the python-based message decoder
    * Using the Aceinna Navigation Studio to capture data

