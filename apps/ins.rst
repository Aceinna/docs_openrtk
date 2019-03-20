
GPS/INS App
===========

The INS APP supports all of the features and operating modes of the
VG/AHRS APP, and it includes additional capability of interfacing
with an external GPS receiver and associated software running on the
processor, for the computation of navigation information as well as
orientation information. The APP name, GPS/INS APP, stands for Inertial
Navigation System, and it is indicative of the navigation reference
functionality that APP provides by outputting inertially-aided
navigation information (Latitude, Longitude, and Altitude),
inertially-aided 3-axis velocity information, as well as heading, roll,
and pitch measurements, in addition to digital IMU data.

At a fixed N rate, the INSx81ZA continuously maintains the digital
IMU data; the dynamic roll, pitch, and heading data; as well as the
navigation data. As shown in the software block the calibrated IMU data is passed
into an “Integration to Orientation” block. The “Integration to
Orientation” block integrates body frame sensed angular rate to
orientation at a fixed N times per second. For improved accuracy and to avoid
singularities when dealing with the cosine rotation matrix, a quaternion
formulation is used in the algorithm to provide attitude propagation.
Following the integration to orientation block, the body frame
accelerometer signals are rotated into the NED level frame and are
integrated to velocity. At this point, the data is blended with GPS
position data, and output as a complete navigation solution.

As shown in the diagram, the Integration to Orientation and the
Integration to Velocity signal processing blocks receive drift
corrections from the Extended Kalman Filter (EKF) drift correction
module. The drift correction module uses data from the aiding sensors,
when they are available, to correct the errors in the velocity,
attitude, and heading outputs. Additionally, when aiding sensors are
available corrections to the rate gyro and accelerometers are performed.

The INS APP blends GPS derived heading and accelerometer measurements
into the EKF update depending on the health and status of the associated
sensors. If the GPS link is lost or poor, the Kalman Filter solution
stops tracking accelerometer bias, but the algorithm continues to apply
gyro bias correction and provides stabilized angle outputs. The EKF
tracking states are reduced to angles and gyro bias only. The
accelerometers will continue to integrate velocity, however,
accelerometer noise, bias, and attitude error will cause the velocity
estimates to start drifting within a few seconds. The attitude tracking
performance will degrade, the heading will freely drift, and the filter
will revert to the VG only EKF formulation. The UTC packet
synchronization will drift due to internal clock drift.

The status of GPS signal acquisition can be monitored from the
hardwareStatus BIT in INS Apps Built
in Test. From a cold start, it typically takes 40 seconds for GPS to
lock. The actual lock time depends on the antenna’s view of the sky and
the number of satellites in view.

The processor performs time-triggered trajectory propagation at 100Hz
and will synchronize the sensor sampling with the GPS UTC (Universal
Coordinated Time) second boundary when available.

As with the AHRS/VG APP, the algorithm has two major phases of
operation. Immediately after power-up, the INS APP uses the
accelerometers and magnetometers to compute the initial roll, pitch and
yaw angles. The roll and pitch attitude will be initialized using the
accelerometer’s reference of gravity, and yaw will be initialized using
the leveled magnetometers X and Y axis reference of the earth’s magnetic
field. During the first 60 seconds of startup, the INS APP should
remain approximately motionless in order to properly initialize the rate
sensor bias. The initialization phase lasts approximately 60 seconds,
and the initialization phase can be monitored in the softwareStatus BIT
transmitted by default in each measurement packet. After the
initialization phase, the INS APP operates with lower levels of
feedback (also referred to as EKF gain) from the GPS, accelerometers,
and magnetometers.

.. note:: 

    For proper operation, the INS APP relies on magnetic field readings
    from its internal 3-axis magnetometer. The INS APP must be installed
    correctly and calibrated for hard-iron and soft iron effects to avoid
    any system performance degradation. See section `3.4.1 <\l>`__ for
    information and tips regarding installation and calibration and why
    magnetic calibration is necessary. Please review this section of the
    manual before proceeding to use the INS APP

.. note::

    For optimal performance the INS APP utilizes GPS readings from an
    external GPS receiver. The GPS receiver requires proper antennae
    installation for operation. See section `2.1.4 <\l>`__ for information
    and tips regarding antenna installation.

.. note::

    The OpenIMU300Rx (RI and RA) do not provide inputs for GPS or other
    external sensors, so they can't support the INS App.

.. contents:: Contents
    :local:

