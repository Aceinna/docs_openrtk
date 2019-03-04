#################################
Building Your First Applications
#################################

.. contents:: Contents
    :local:

.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>

.. toctree::
    :maxdepth: 2


*********
Overview
*********

To get you acquainted with the OpenIMU environment, let's walk through the development of the
following applications:

    1. Framework application
    2. An Inertial-Measurement Unit (IMU)
    3. A Static-Leveler
    4. VG/AHRS
    5. INS (under development) 

.. note:: The OpenIMU300xI (RI, AI) units do not provide any inputs for GPS or other external sensors, so the INS application is not applicable to those units. 

**Framework**

All the examples that follow are based on the Framework application that ships installed on the
OpenIMU.  This framework serves as a blank-slate upon which user-applications can be built.  The
descriptions that follow provide guidance on getting started, specifically on where in the
framework to place sensor reads, algorithm code, and serial messaging output functions (among other
items).

For example, in the Framework application, all data acquisition functions (sensor-buffer getters)
are placed in *inertialAndPositionDataProcessing()*.  Depending upon your application, getters of
interest can be kept while unused getters can be removed.  For instance, in the Leveler
application, only accelerometer data is of interest.  No call is made for rate-sensor or
magnetometer data so the getters associated with this data can be removed.

To begin development of the IMU, Static-Leveler, or VG/AHRS application, download the application
code related to the application being described (IMU, Leveler, etc).  Follow the description to get an
understanding how all the parts fit together.  When you are ready to create your own application,
save the Framework application as something descriptive of your project and modify the code to suit
your needs.


**IMU**

An IMU refers to a device that returns calibrated, inertial-sensor data.  This application forms
the backbone of all other inertial-sensor based applications as each requires measurements to
generate results.


**Static-Leveler**

A static-leveler uses accelerometer readings to measure the local gravity-field and compute the
two-axis attitude (roll and pitch angles) of a body relative to the local-level frame.


**VG/AHRS**

The Vertical Gyro (VG) and Attitude and Heading Reference System (AHRS) application 
uses rate-sensors, accelerometers, and (in the case of an AHRS) magnetometers to
compute the attitude and heading of a body in space.  Rate-sensors are used to propagate the
attitude forward in time at high data-rates (DR) while accelerometers and magnetometers act as
references, correcting for rate-sensor biases and attitude errors.


**INS**

The Inertial Navigation System application description is under development...


******************
Application Links
******************

.. toctree::
    :maxdepth: 2

    tutorial/IMU_App
    tutorial/Leveler_App
    tutorial/VG_AHRS_App
    tutorial/INS_App


.. links-placeholder




    