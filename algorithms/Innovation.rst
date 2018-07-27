*******************************
Innovation / Measurement Error
*******************************

.. toctree::
    :maxdepth: 4

.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


Overview
=========

The innovation (measurement error) is formed from the sensor measurements and the predicted states.
As the measurements and the system states are often not the same, one or the other needs to be
transformed into the mesurement.  In the case of this algorithm, the state consists of an attitude
quaternion, NED-velocity, and NED-position.  The measurement come from accelerometer readings, GPS
latitude/longitude/altitude measurements, and horizontal/vertical velocities along with
ground-track.
`Table 2 <Sensors.html#id4>`__)
In this case either the states need to be converted to match the measurements or vice-versa.

    
    


Innovation (Measurement Error):
=================================

Once the measurements vectors are formed, the innovation (measurement error), :math:`\vec{\nu}_{k}`,
is computed:

.. math::

    \vec{\nu}_{k} = \vec{z}_{k} - \vec{h}_{k}


This result is used in the update stage of the EKF to generate the state error,
:math:`{\Delta\vec{x}}_{k}`, given the Kalman gain matrix.



stateenvironment of the system  and uses the available sensor information as follows:

    1) Accelerometers “level” the system (used to compute :math:`{^{⊥}}{\phi}{_{meas}^{B}}` and
       :math:`{^{⊥}}{\theta}{_{meas}^{B}}`) FN

    2) Magnetometers and/or GPS heading information align the ⊥-frame with true or magnetic north
       (:math:`{^{N}}{\psi}{^{⊥}}`)

    3) GPS position and velocity measurements update the position and velocity estimates
       (:math:`\vec{r}^{N}` and :math:`\vec{v}^{N}`)


Based upon these steps, the measureme