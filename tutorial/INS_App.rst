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

The INS example application performs the following functions:

    * Sets the default OpenIMU configuration
    * Acquires sensor data  - acceleration, angular-rate, local magnetic-field, GPS, and sensor temperature data
    * Populates the output data structure
    * Generates and sends the following output message to the UART - the output message description is To Be Provided
