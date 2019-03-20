***************************************
Inertial Navigation System Application
***************************************

.. contents:: Contents
    :local:
    
.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


The INS APP supports all of the features and operating modes of the
VG/AHRS App.  In addition it includes the capability of interfacing
with an external GPS receiver and associated software running on the
processor, allowing computation of navigation information as well as
orientation information. The application name, GPS/INS APP, stands for GPS Inertial
Navigation System, and it is indicative of the navigation reference
functionality that application provides by outputting inertially-aided
navigation information (Latitude, Longitude, and Altitude),
inertially-aided 3-axis velocity information, as well as heading, roll,
and pitch measurements, in addition to digital IMU data.

The mathematics behind the algorithm are more complicated than the math associated with
the VG/AHRS application.  The full description is not discussed here, as the 
complete formulation is provided in the "Ready-to-use Applications" section.  
Just as for the IMU, Static-Leveler, and VG/AHRS applications, the
development of the GPS/INS application is broken up into a series of steps:

    * Setting the default OpenIMU configuration
    * Acquiring sensor data
    * Executing the INS algorithm
    
          * Obtaining sensor data within the algorithm function
          * Executing the algorithm
          * Populating the output data structure within the algorithm function
          
    * Using the serial-debugger
    * Generating serial output messages
    * Setting up and using the python-based message decoder
    * Using the Aceinna Navigation Studio to capture data

