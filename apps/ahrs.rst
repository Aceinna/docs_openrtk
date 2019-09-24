
AHRS/VG Dynamic Attitude App
============================


The Attitude and Heading Reference System (AHRS) and Vertical Gyro (VG) application 
supports all of the features and operating modes of the
IMU APP, and it links in additional internal software, running on the
processor, for the computation of dynamic roll, pitch. 
In addition to the Roll,Pitch and IMU data, the dynamic heading measurement is optionally stabilized 
using the 3-axis magnetometer as a magnetic north reference.  Roll, Pitch
measurements are often referred to as "VG" or Vertical Gyro measurements.
When heading stabilized by a magnetometer is added, the solution is often referred to
as an "AHRS" or Attitude Heading Reference System.  Hence the name of tis APP
is AHRS/VG APP.

At a fixed 200Hz rate, the VG/AHRS APP continuously maintains the digital
IMU data as well as the dynamic roll, pitch, and heading. As shown in diagram
after the Sensor Calibration Block, the IMU data is
passed to the Integration to Orientation block. The Integration to
Orientation block integrates body frame sensed angular rate to
orientation at a fixed 200 times per second within all of the OpenIMU
Series products. For improved accuracy and to avoid singularities when
dealing with the cosine rotation matrix, a quaternion formulation is
used in the algorithm to provide attitude propagation.

As also shown in the software block diagram, the Integration to
Orientation block receives drift corrections from the Extended Kalman
Filter or Drift Correction Module. In general, rate sensors and
accelerometers suffer from bias drift, misalignment errors, acceleration
errors (g-sensitivity), nonlinearity (square terms), and scale factor
errors. The largest error in the orientation propagation is associated
with the rate sensor bias terms. The Extended Kalman Filter (EKF) module
provides an on-the-fly calibration for drift errors, including the rate
sensor bias, by providing corrections to the Integration to Orientation
block and a characterization of the gyro bias state. In the AHRS/VG APP,
the internally computed gravity reference vector and the distortion
corrected magnetic field vector provide an attitude and a heading
reference measurement for the EKF when the unit is in quasi-static
motion to correct roll, pitch, and heading angle drift and to estimate
the X, Y and Z gyro rate bias. The AHRS/VG APP adaptively tunes the EKF
feedback gains in order to best balance the bias estimation and attitude
correction with distortion free performance during dynamics when the
object is accelerating either linearly (speed changes) or centripetally
(false gravity forces from turns). Because centripetal and other dynamic
accelerations are often associated with yaw rate, the AHRS/VG APP
maintains a low-passed filtered yaw rate signal and compares it to the
turnSwitch threshold field (user adjustable). When the user platform
exceeds the turnSwitch threshold yaw rate,
the AHRS/VG APP lowers the feedback gains from the accelerometers to allow
the attitude estimate to coast through the dynamic situation with
primary reliance on angular rate sensors. This situation is indicated by
the softwareStatus - turnSwitch status flag. Using the turn switch
maintains better attitude accuracy during short-term dynamic situations,
but care must be taken to ensure that the duty cycle of the turn switch
generally stays below 10% during the vehicle mission. A high turn switch
duty cycle does not allow the system to apply enough rate sensor bias
correction and could allow the attitude estimate to become unstable.

The AHRS/VG APP algorithm also has two major phases of operation. The first phase of
operation is the high-gain initialization phase. During the
initialization phase, the OpenIMU unit is expected to be stationary or
quasi-static so the EKF weights the accelerometer gravity reference and
Earth’s magnetic field reference heavily in order to rapidly estimate
the X, Y, and Z rate sensor bias, and the initial attitude and heading
of the AHRSx81ZA. The initialization phase lasts approximately 60
seconds, and the initialization phase can be monitored in the
softwareStatus BIT transmitted by default in each measurement packet.
After the initialization phase, the AHRS/VP APP operates with lower levels
of feedback (also referred to as EKF gain) from the accelerometers and
magnetometers to continuously estimate and correct for roll, pitch, and
heading (yaw) errors, as well as to estimate X, Y, and Z rate sensor
bias.

The Definitions of The Output Packets of The VG/AHRS App
----------------------------------------------------------------

"a1" packet
^^^^^^^^^^^^

The default VG/AHRS app output packet type is "a1", and it is defined in the following two tables.

    +----------------------+-------------+--------+----------------+-------------+
    |   ('a1' = 0x6131)    |             |        |                |             |
    +----------------------+-------------+--------+----------------+-------------+
    | Preamble             | Packet Type | Length | Payload        | Termination |
    +----------------------+-------------+--------+----------------+-------------+
    | 0x5555               | 0x6131      |  47    |                | <CRC (U2)>  |
    +----------------------+-------------+--------+----------------+-------------+

    Payload:

    +-----------+--------------------------+-----------+-----------+
    | Byte      | Name                     | Format    | Notes     |
    | Offset    |                          |           |           |
    +-----------+--------------------------+-----------+-----------+
    |  0        || System Timer of         | U4        || LSB First|
    |           || sensors sampling        |           || msec     |
    +-----------+--------------------------+-----------+-----------+
    |  4        || Above timer converted   | D         || LSB First|
    |           || to a double type        |           || second   |
    +-----------+--------------------------+-----------+-----------+
    |  12       | Roll                     | F4        || LSB First|
    |           |                          |           || deg      |
    +-----------+--------------------------+-----------+-----------+
    |  16       | Pitch                    | F4        || LSB First|
    |           |                          |           || deg      |
    +-----------+--------------------------+-----------+-----------+
    |  20       | corrected X gyro         | F4        || LSB First|
    |           |                          |           || deg/s    |
    +-----------+--------------------------+-----------+-----------+
    |  24       | corrected Y gyro         | F4        || LSB First|
    |           |                          |           || deg/s    |
    +-----------+--------------------------+-----------+-----------+
    |  28       | corrected Z gyro         | F4        || LSB First|
    |           |                          |           || deg/s    |
    +-----------+--------------------------+-----------+-----------+
    |  32       | X Accel                  | F4        || LSB First|
    |           |                          |           || m/s/s    |
    +-----------+--------------------------+-----------+-----------+
    |  36       | Y Accel                  | F4        || LSB First|
    |           |                          |           || m/s/s    |
    +-----------+--------------------------+-----------+-----------+
    |  40       | Z Accel                  | F4        || LSB First|
    |           |                          |           || m/s/s    |
    +-----------+--------------------------+-----------+-----------+
    |  44       | Operation mode [1]_      | U1        | LSB First |
    |           |                          |           |           |
    +-----------+--------------------------+-----------+-----------+
    |  45       | Linear accel switch [2]_ | U1        | LSB First |
    |           |                          |           |           |
    +-----------+--------------------------+-----------+-----------+
    |  46       | Turn switch [3]_         | U1        | LSB First |
    |           |                          |           |           |
    +-----------+--------------------------+-----------+-----------+

"a2" packet
^^^^^^^^^^^^

If you want to output the yaw angle, you can chooose the "a2" packet. For the VG app, the yaw angle is from integrating the gyro rate,
and for the AHRS app, the yaw angle gets corrected by magnetometer measurements.

    +----------------------+-------------+--------+----------------+-------------+
    |   ('a2' = 0x6132)    |             |        |                |             |
    +----------------------+-------------+--------+----------------+-------------+
    | Preamble             | Packet Type | Length | Payload        | Termination |
    +----------------------+-------------+--------+----------------+-------------+
    | 0x5555               | 0x6132      |  48    |                | <CRC (U2)>  |
    +----------------------+-------------+--------+----------------+-------------+

    Payload:

    +-----------+--------------------------+-----------+-----------+
    | Byte      | Name                     | Format    | Notes     |
    | Offset    |                          |           |           |
    +-----------+--------------------------+-----------+-----------+
    |  0        || System Timer of         | U4        || LSB First|
    |           || sensors sampling        |           || msec     |
    +-----------+--------------------------+-----------+-----------+
    |  4        || Above timer converted   | D         || LSB First|
    |           || to a double type        |           || second   |
    +-----------+--------------------------+-----------+-----------+
    |  12       | Roll                     | F4        || LSB First|
    |           |                          |           || deg      |
    +-----------+--------------------------+-----------+-----------+
    |  16       | Pitch                    | F4        || LSB First|
    |           |                          |           || deg      |
    +-----------+--------------------------+-----------+-----------+
    |  20       | Yaw                      | F4        || LSB First|
    |           |                          |           || deg      |
    +-----------+--------------------------+-----------+-----------+
    |  24       | corrected X gyro         | F4        || LSB First|
    |           |                          |           || deg/s    |
    +-----------+--------------------------+-----------+-----------+
    |  28       | corrected Y gyro         | F4        || LSB First|
    |           |                          |           || deg/s    |
    +-----------+--------------------------+-----------+-----------+
    |  32       | corrected Z gyro         | F4        || LSB First|
    |           |                          |           || deg/s    |
    +-----------+--------------------------+-----------+-----------+
    |  36       | X Accel                  | F4        || LSB First|
    |           |                          |           || m/s/s    |
    +-----------+--------------------------+-----------+-----------+
    |  40       | Y Accel                  | F4        || LSB First|
    |           |                          |           || m/s/s    |
    +-----------+--------------------------+-----------+-----------+
    |  44       | Z Accel                  | F4        || LSB First|
    |           |                          |           || m/s/s    |
    +-----------+--------------------------+-----------+-----------+

"e1" packet
^^^^^^^^^^^^

If you further want to output the magnetometer measurements, you can chooose the "e1"
packet.

    +----------------------+-------------+--------+----------------+-------------+
    |   ('e1' = 0x6531)    |             |        |                |             |
    +----------------------+-------------+--------+----------------+-------------+
    | Preamble             | Packet Type | Length | Payload        | Termination |
    +----------------------+-------------+--------+----------------+-------------+
    | 0x5555               | 0x6531      |  75    |                | <CRC (U2)>  |
    +----------------------+-------------+--------+----------------+-------------+

    Payload:

    +-----------+--------------------------+-----------+-----------+
    | Byte      | Name                     | Format    | Notes     |
    | Offset    |                          |           |           |
    +-----------+--------------------------+-----------+-----------+
    |  0        || System Timer of         | U4        || LSB First|
    |           || sensors sampling        |           || msec     |
    +-----------+--------------------------+-----------+-----------+
    |  4        || Above timer converted   | D         || LSB First|
    |           || to a double type        |           || second   |
    +-----------+--------------------------+-----------+-----------+
    |  12       | Roll                     | F4        || LSB First|
    |           |                          |           || deg      |
    +-----------+--------------------------+-----------+-----------+
    |  16       | Pitch                    | F4        || LSB First|
    |           |                          |           || deg      |
    +-----------+--------------------------+-----------+-----------+
    |  20       | Yaw                      | F4        || LSB First|
    |           |                          |           || deg      |
    +-----------+--------------------------+-----------+-----------+
    |  24       | X Accel                  | F4        || LSB First|
    |           |                          |           || g        |
    +-----------+--------------------------+-----------+-----------+
    |  28       | Y Accel                  | F4        || LSB First|
    |           |                          |           || g        |
    +-----------+--------------------------+-----------+-----------+
    |  32       | Z Accel                  | F4        || LSB First|
    |           |                          |           || g        |
    +-----------+--------------------------+-----------+-----------+
    |  36       | X gyro                   | F4        || LSB First|
    |           |                          |           || deg/s    |
    +-----------+--------------------------+-----------+-----------+
    |  40       | Y gyro                   | F4        || LSB First|
    |           |                          |           || deg/s    |
    +-----------+--------------------------+-----------+-----------+
    |  44       | Z gyro                   | F4        || LSB First|
    |           |                          |           || deg/s    |
    +-----------+--------------------------+-----------+-----------+
    |  48       | X gyro bias              | F4        || LSB First|
    |           |                          |           || deg/s    |
    +-----------+--------------------------+-----------+-----------+
    |  52       | Y gyro bias              | F4        || LSB First|
    |           |                          |           || deg/s    |
    +-----------+--------------------------+-----------+-----------+
    |  56       | Z gyro bias              | F4        || LSB First|
    |           |                          |           || deg/s    |
    +-----------+--------------------------+-----------+-----------+
    |  60       | X magnetometer           | F4        || LSB First|
    |           |                          |           || Gauss    |
    +-----------+--------------------------+-----------+-----------+
    |  64       | Y magnetometer           | F4        || LSB First|
    |           |                          |           || Gauss    |
    +-----------+--------------------------+-----------+-----------+
    |  68       | Z magnetometer           | F4        || LSB First|
    |           |                          |           || Gauss    |
    +-----------+--------------------------+-----------+-----------+
    |  72       | Operation mode [1]_      | U1        | LSB First |
    |           |                          |           |           |
    +-----------+--------------------------+-----------+-----------+
    |  73       | Linear accel switch [2]_ | U1        | LSB First |
    |           |                          |           |           |
    +-----------+--------------------------+-----------+-----------+
    |  74       | Turn switch [3]_         | U1        | LSB First |
    |           |                          |           |           |
    +-----------+--------------------------+-----------+-----------+
    
.. [1] Operation mode of the algorithm. 0 for waiting for the system to stabilize, 1 for initialzing attiude,
        2 and 3 for VG/AHRS mode, and 4 for INS mode. Please refer to the source code for details.
.. [2] 0 if linear acceleration is detected, 1 if no linear acceleration. Please refer to the source code for details.
.. [3] Indicate if the filtered yaw rate exceeds the turn switch threshold. 1 yes, 0 no. Please refer to the source code for details.


.. note:: 

    In AHRS mode for proper operation of the stabilized heading measurement, the AHRS/VG
    APP uses information from the internal 3-axis digital magnetometer. The AHRS APP must be installed
    correctly and calibrated for hard-iron and soft iron effects to avoid
    any system performance degradation. See section XXX for
    information and tips regarding installation and calibration and why
    magnetic calibration is necessary. Please review this section of the
    manual before proceeding to using the heading data



.. contents:: Contents
    :local:

