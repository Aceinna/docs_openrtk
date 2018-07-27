******************************************************************
Vertical-Gyro / Attitude and Heading Reference System Application
******************************************************************

.. contents:: Contents
    :local:


The Vertical-Gyro (VG) / Attitude and Heading Reference System (AHRS) application enables the
OpenIMU hardware to fuse inertial-sensor information (accelerometers, rate-sensors, and — for the
AHRS — magnetometers) to generate an attitude solution.  The solution makes use of the high
data-rate (DR) rate-sensor output to propagate the attitude forward in time while using the
accelerometers and magnetometers as references to correct for estimated rate-bias errors and
attitude-errors at a lower DR.


The mathematics behind the algorithm are quite a bit more complicated than the math associated with
the `Static-Leveler <Leveler/LevelerAlgorithm.html#static-leveler-algorithm>`__.  The full
description is not discussed here.  However, the complete formulation is provided in the
`EKF Algorithms <../algorithms.html#ekf-algorithms>`__ section.  Just as for the
`IMU <IMU_App.html#inertial-measurement-unit-application>`__ and
`Static-Leveler <Leveler_App.html#static-leveler-application>`__ applications, the
development of the VG/AHRS application is broken up into a series of steps:

    * Acquiring sensor data
    * Creating an algorithm
    
          * Obtaining sensor data within the algorithm function
          * Executing the algorithm
          * Populating the output data structure within the algorithm function
          
    * Using the serial-debugger
    * Generating serial output messages
    * Setting the default OpenIMU configuration
    * Setting up and using the python-based message decoder
    * Using the Aceinna Navigation Studio to capture data


Application development is broken up into a series of sections that build upon one another,
as follows:

.. toctree::
    :maxdepth: 1

    VG_AHRS/DataAcquisition
    VG_AHRS/Requirements
    VG_AHRS/AHRS_Algorithm
    VG_AHRS/SerialDebugger
    VG_AHRS/SerialMessaging
    VG_AHRS/DefaultConfiguration
    VG_AHRS/PythonMessageDecoder
    VG_AHRS/NavigationStudio

