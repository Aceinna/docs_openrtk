################################
Building Your First Custom Apps
################################

.. contents:: Contents
    :local:


*********
Overview
*********

To get you acquainted with the OpenIMU environment, we will walk you through the development of a
few simple applications:

    1. An Inertial-Measurement Unit (IMU)
    2. A static-leveler app
    3. VG/AHRS (under development)
    4. INS (under development)


**IMU**

The first application, an IMU refers to a device that returns calibrated, inertial-sensor data.
This application forms the backbone of all other inertial-sensor based applications as each
requires measurements to generate results.

**Static-Leveler**

A static-leveler uses accelerometer readings to measure the local gravity-field and compute the
two-axis attitude (roll and pitch angles) of a body relative to the local-level frame.

**VG/AHRS**

Under development...

**INS**

Under development...


****************
Applications
****************

.. toctree::
    :maxdepth: 1

    tutorial/IMU_App
    tutorial/Leveler_App
    tutorial/VG_AHRS_App
    tutorial/INS_App


.. links-placeholder




    