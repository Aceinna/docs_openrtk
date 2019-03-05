********
Sensors
********

.. contents:: Contents
    :local:

.. role::  raw-html(raw)
    :format: html

.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


Various sensors are used to obtain the information needed to estimate the position, velocity, and
attitude of a system (`Table 2 <Sensors.html#id4>`__) .  Measurements from these sensors, taken
over time, are combined using an Extended Kalman Filter (EKF) to arrive at an estimates that are
more accurate or more timely than ones based on any single measurement.

.. table:: **Table 2: Inertial Sensors and Measurement Type**
    :widths: 15 25 60

    +-----------------+-------------------+-------------------------------------------------------------+
    | **Measurement** | **Sensor**        |  **Description**                                            |
    +=================+===================+=============================================================+
    | Position        || GPS              || GPS provides position (Latitude/Longitude/Altitude) and    |
    |                 ||                  || supplemental information (like standard deviation) to      |
    |                 ||                  || the algorithm.  This is used to update the errors in the   |
    |                 ||                  || position (integrated velocity) estimate.                   |
    +-----------------+-------------------+-------------------------------------------------------------+
    | Velocity        || 1) Accelerometer || Accelerometers provide the high DR/low-noise signal        |
    |                 || 2) GPS           || that is integrated to get high DR velocity information.    |
    |                 ||                  || GPS provides velocity and supplemental information to      |
    |                 ||                  || the algorithm (velocity, heading, latency, etc), which is  |
    |                 ||                  || used to update errors due to integration of the            |
    |                 ||                  || accelerometer signal (in particular, to estimate the       |
    |                 ||                  || accelerometer bias).                                       |
    +-----------------+-------------------+-------------------------------------------------------------+
    | Roll/Pitch      || 1) Angular-Rate  || Angular-rate sensors provide the high DR/low-noise         |
    |                 ||    Sensor        || signal that is integrated to get high DR attitude          |
    |                 || 2) Accelerometer || information.  Accelerometers are used as a gravity         |
    |                 ||                  || reference to update errors due to integration of the rate- |
    |                 ||                  || sensor signal (in particular, to estimate the rate-sensor  |
    |                 ||                  || bias).                                                     |
    +-----------------+-------------------+-------------------------------------------------------------+
    | Heading         || 1) Angular-Rate  || Angular-rate sensors provide the high DR/low-noise         |
    |                 ||    Sensor        || signal that is integrated to get high DR heading           |
    |                 || 2) Magnetometer  || information.  Magnetometers are used as a north-           |
    |                 || 3) GPS           || reference to update errors due to integration of the rate- |
    |                 ||                  || sensor signal (in particular, to estimate the z-axis rate- |
    |                 ||                  || sensor bias).  GPS also provides heading information,      |
    |                 ||                  || which is used in lieu of magnetometer readings and can     |
    |                 ||                  || be more accurate (less prone to disturbances) than the     |
    |                 ||                  || magnetometer but available less often.                     |
    +-----------------+-------------------+-------------------------------------------------------------+


Other sensors, such as odometers, barometers, cameras, etc., may be incorporated into the EKF
formulation to get improved results.  However, incorporating data from any additional sensors would
require a reformulation of the algorithm presented here.


Inertial sensors measure the true motion and attitude of a system, corrupted by bias, noise, and
external influences.  For instance, the accelerometer signal is a combination of platform motion
and gravity\ [#aDueToGravity]_, as well as sensor bias and noise.  Simplified equations for the
three sensors are provided below:


.. math::

    \vec{\omega}_{meas} &= \vec{\omega}_{true} + \vec{\omega}_{bias} + \vec{\omega}_{noise}\\
    {\hspace{5mm}} \\
    \vec{a}_{meas} &= \vec{a}_{motion} + \vec{a}_{grav} + \vec{a}_{bias} + \vec{a}_{noise}\\
    {\hspace{5mm}} \\
    \vec{m}_{meas} &= \vec{b}_{motion} + \vec{m}_{bias} + \vec{m}_{noise}


Items, such as misalignment, cross-coupling, etc. are ignored in this formulation they are
accounted for during system calibration.


Additionally, sensor bias can be broken down further.  In this paper, bias is modeled as a
constant offset plus random drift:

.. math::

    \vec{\omega}_{bias} = \vec{\omega}_{offset} + \vec{\omega}_{drift}


The magnetic field vector, |bVec|, may be corrupted by hard and soft-iron sources present in the
system in which the part is installed.  Hard and soft-iron effects can be estimated by performing
a “magnetic-alignment”\ [#magAlign]_ procedure once installed in the end-user’s system.  The
equations relating the hard and soft-iron effects\ [#ironEffects]_ on the measured magnetic field
is:

.. math::

    \vec{m}_{meas} = {\begin{pmatrix} {R_{SI} \cdot S_{SI} \cdot {R_{SI}}^{T}} \end{pmatrix}}^{-1} \cdot \vec{b} + \vec{m}_{HI} + \vec{m}_{bias} + \vec{m}_{noise}


Where |R_SI| and |S_SI| represent the rotation and scaling of the magnetic-field, |bVec|, due to
soft-iron effects; |mHI| is the bias change in the magnetic-field due to hard-iron in the system.
Sensor gain is measured during the calibration process with the system at room temperature; it does
not vary much over temperature.  Sensor bias, however, is strongly linked to temperature.  The
calibration process measures bias over temperature (from -40° C to +85° C).  The temperature effect
on the magnetometer is “ratiometric”; the unitized magnetic-field vector is unaffected by
temperature.


Finally, and most importantly for the Extended Kalman Filter application, all sensor noise signals
are assumed to be white, Gaussian, stationary, and independent.  This implies that a sensor’s noise
characteristics are:

    * zero-mean (:math:`\mu = 0`)

    * distributed according to a normal distribution with variance :math:`\sigma^2`

    * constant over time (:math:`\sigma^2 \ne f(t)`)

    * uncorrelated with other signals (:math:`E{ \begin{bmatrix} { {\begin{pmatrix} {\sigma_{\omega,x} - E[\sigma_{\omega,x}]} \end{pmatrix}} \cdot {\begin{pmatrix} {\sigma_{\omega,y} - E[\sigma_{\omega,y}]} \end{pmatrix}} } \end{bmatrix} } = 0`\ )


The formulation of the covariance matrices relies heavily on these assumption.

.. note::

    The process-noise vectors, :math:`\vec{w}`, result from sensor noise transmission through the
    individual state-transition models, described in the sections to come.


.. |bVec| replace:: :math:`\vec{b}`

.. |R_SI| replace:: :math:`R_{SI}`
.. |S_SI| replace:: :math:`S_{SI}`
.. |mHI|  replace:: :math:`\vec{m}_{HI}`

.. [#aDueToGravity] Due to the way the accelerometer measures acceleration, gravity appears like a
                    deceleration and, as such, :math:`\vec{a}_{grav} = -\vec{g}`\ .  This is
                    gravity deflecting the proof-mass in the direction of the gravity vector; such
                    a deflection caused solely by acceleration would require the body to accelerate
                    in the negative direction.

.. [#magAlign] During a magnetic alignment maneuver, the magnetic measurements are recorded as the
               system rotates (about its z-axis) through 360 deg.  Upon completion of the maneuver,
               a best-fit ellipse is determined and used to model the hard and soft-iron
               distortions in the system (described later).

.. [#ironEffects] In general you want the magnetic sensor to be in as magnetically clean a location
                  as possible.  Even by correcting for hard and soft-iron using this relationship,
                  large hard and soft-iron errors lead to progressively worse solutions.
