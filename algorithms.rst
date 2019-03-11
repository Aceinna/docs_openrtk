###############
EKF Algorithms
###############

.. contents:: Contents
    :local:

This section develops the equations that form the basis of an Extended Kalman Filter (EKF), which
calculates position, velocity, and orientation of a body in space\ [#caveat]_.  In a VG, AHRS, or
INS\ [#ProdDef]_ application, inertial sensor readings are used to form high data-rate (DR)
estimates of the system states while less frequent or noisier measurements (GPS and inertial
sensors) act as references to correct errors in the system.

In addition to deriving the EKF equations, this description presents a measurement model based on
Euler angles, which result from accelerometers, magnetometers, and GPS readings.  Following that it
describes implementations that result in improved solutions under both static and dynamic
conditions.  Finally, a series of examples illustrate existing VG, AHRS, and INS algorithms.


The algorithm development description is broken up into a series of sections that build upon one
another, as follows:

.. toctree::
    :maxdepth: 3

    algorithms/CoordFrames
    algorithms/AttitudeParameters
    algorithms/Sensors
    algorithms/KalmanFilter
    algorithms/StateTransitionModels
    algorithms/ProcessModels
    algorithms/MeasurementModel
    algorithms/MeasurementVector
    algorithms/Innovation
    algorithms/MagneticAlignment
    algorithms/References


.. [#caveat] This discussion presupposes a certain amount of knowledge.  Details related to
             differential equations, linear algebra, multi-variable calculus, stochastic
             processes, etc. are not described.

.. [#ProdDef] A VG uses rate-sensors and accelerometers to estimate roll and pitch.  An AHRS
              incorporates magnetometer readings to the VG to estimate heading.  An INS adds GPS
              messages to the VG or AHRS to estimate position and velocity or provide a way to
              estimate heading without magnetometers.
