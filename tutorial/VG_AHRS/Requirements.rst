********************
App Requirements
********************

.. contents:: Contents
    :local:
    
.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


Development of an application requires system level requirements that describe the purpose of the
application.  These requirements provide a guide for product development.

For the Vertical-Gyro application, the following were selected:

    1. Measure roll and pitch angles of an object with a static-accuracy better than 0.10 [deg]
    2. Compute the attitude at 200 Hz
    3. Provide an output message consisting of the following:

       a. A relative time measurement (integer and decimal)
       b. Attitude consisting of roll and pitch angles
       c. Acceleration readings in :math:`[{m / s^2}]`
       d. Corrected rate-sensor readings in :math:`[{deg / s}]`.  Corrected readings have the
          estimated rate-sensor bias removed from the measurement.
       e. Uncorrected rate-sensor readings in :math:`[{deg / s}]`
       
For the Attitude and Heading Reference System application, the following were selected:

    1. Measure roll and pitch angles of an object with a static-accuracy better than 0.10 [deg]
    2. Measure heading angle of an object with a static-accuracy better than 1.0 [deg]
    3. Compute the attitude at 200 Hz
    4. Provide an output message consisting of the following:

       a. A relative time measurement (integer and decimal)
       b. Attitude consisting of roll, pitch, and heading angles
       c. Acceleration readings in :math:`[{m / s^2}]`
       d. Corrected rate-sensor readings in :math:`[{deg / s}]`.  Corrected readings have the
          estimated rate-sensor bias removed from the measurement.
       e. Uncorrected rate-sensor readings in :math:`[{deg / s}]`
       f. Magnetometer readings in :math:`[{G}]`

