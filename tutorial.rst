#################################
Building Your First Applications
#################################

.. contents:: Contents
    :local:


*********
Overview
*********

To get you acquainted with the OpenIMU environment, let's walk through the development of the
following applications:

    1. Framwork application
    2. An Inertial-Measurement Unit (IMU)
    3. A Static-Leveler
    4. VG/AHRS
    5. INS (under development)


**Framework**

All the examples that follow are based on the Framework application that ships installed on the
OpenIMU.  This framework serves as a guided, blank-slate.  Descriptions provide guidance on where
to place sensor reads, algorithm code, and serial messaging output functions (among other items).

For example, data acquisition functions (sensor-buffer getters) are placed in
*inertialAndPositionDataProcessing()*.  The getters of interest can be used and the others removed.
For instance, in the Leveler application, only the accelerometer data is of interest.  So no call
is made for rate-sensor or magnetometer data and the getters associated with this data can be
removed.

To begin development of the IMU, Static-Leveler, or VG/AHRS application, rename the Framework
application as something that is descriptive of your project and modify the code to suit your
needs.  You can do this while walking through the following application examples.


**IMU**

An IMU refers to a device that returns calibrated, inertial-sensor data.  This application forms
the backbone of all other inertial-sensor based applications as each requires measurements to
generate results.


**Static-Leveler**

A static-leveler uses accelerometer readings to measure the local gravity-field and compute the
two-axis attitude (roll and pitch angles) of a body relative to the local-level frame.


**VG/AHRS**

VG and AHRS uses rate-sensors, accelerometers, and (in the case of an AHRS) magnetometers to
compute the attitude and heading of a body in space.  Rate-sensors are used to propagate the
attitude forward in time at high data-rates (DR) while accelerometers and magnetometers act as
references, correcting for rate-sensor biases and attitude errors.


**INS**

Under development...


******************
Application Links
******************

.. toctree::
    :maxdepth: 1

    tutorial/IMU_App
    tutorial/Leveler_App
    tutorial/VG_AHRS_App
    tutorial/INS_App


.. links-placeholder




    