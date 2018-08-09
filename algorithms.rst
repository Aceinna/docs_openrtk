###############
EKF Algorithms
###############

.. contents:: Contents
    :local:

This section develops the equations that form the basis of an Extended Kalman Filter (EKF) used to
calculate position, velocity, and orientation of a body in space\ [#caveat]_.  In a VG, AHRS, or
INS\ [#ProdDef]_, inertial sensors are used to form high data-rate (DR) estimates of the system
states while less frequent or noisier measurements (GPS and inertial sensors, which act as
references) aid in correcting errors in the system.

In addition to deriving the equations used in the EKF, this paper describes a measurement model
based on Euler angles resulting from accelerometers, magnetometers, and GPS readings.  Then it
describes implementations that result in improved solutions under both static and dynamic
conditions.

Finally, a series of examples for implementing existing algorithm for VG, AHRS, and INS are
presented.


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
    algorithms/MeasurementVector
    algorithms/MeasurementModel
    algorithms/Innovation
    algorithms/MagneticAlignment
    algorithms/TestArea
    algorithms/References


.. [#caveat] This discussion presupposes a certain amount of knowledge.  Details related to
             differential equations, linear algebra, multi-variable calculus, stochastic
             processes, etc. are not described.

.. [#ProdDef] A VG uses rate-sensors and accelerometers to estimate roll and pitch.  An AHRS adds
              magnetometers to the VG to estimate heading.  An INS adds GPS messages to the VG or
              AHRS to estimate position and velocity or provide a way to estimate heading without
              magnetometers.
