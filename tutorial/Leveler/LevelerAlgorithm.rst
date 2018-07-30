**************************
Static Leveler Algorithm
**************************

.. contents:: Contents
    :local:
    
.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


Mathematical Formulation
==========================

Measurements of roll and pitch angles (rotations of a body about angles around the x and y-axes)
are possible by measuring the local gravity field.  As local gravity is constant the relationship
between the attitude of a body and the gravity-field is deterministic (more detail is found in the
`Measurement Model <../../algorithms/MeasurementModel.html#roll-and-pitch-measurements>`__ section
of `EKF Algorithms <../../algorithms.html#ekf-algorithms>`__).  The formulation for the attitude
(Euler angles) corresponding to the accelerometer signal is:

.. math::

    {^{⊥}}{\phi}{_{meas}^{B}} =atan2(\hat{g}_{my}^{B},\hat{g}_{mz}^{B} )

.. math::

    {^{⊥}}{\theta}{_{meas}^{B}}  =-asin(\hat{g}_{mx}^{B} )

where, :math:`\hat{g}^{B}` is the normalized gravity-field vector.


Individual terms appearing in the equations are:

    * Roll and pitch angle (rotation angles about the x and y-axes): :math:`{^{⊥}}{\phi}{_{meas}^{B}}` and :math:`{^{⊥}}{\theta}{_{meas}^{B}}`

    * Unitized x, y, and z-axis gravity measurements: :math:`\hat{g}_{mx}^{B}`, :math:`\hat{g}_{my}^{B}`, and :math:`\hat{g}_{mz}^{B}`


The unitized gravity measurement is found in two steps:

    1. Acceleration measurements are normalized by dividing the individual axis measurements by the magnitude of the vector measurement:
    
    .. math::

        \hat{a}_{meas}^{B} &= { {\vec{a}_{meas}^B} \over \| {\vec{a}_{meas}^{B}} \|}
    
                           &= { {\vec{a}_{meas}^B} \over \sqrt{ {a_{mx}}^2 + {a_{my}}^2 + {a_{mz}}^2 } }

    
    2. The gravity unit-vector is found by negating the acceleration unit-vector:
    
    .. math::

        \hat{g}_{meas}^{B} = -{\hat{a}_{meas}^B}


Algorithm Development
==========================

Algorithm development, based on these equations, is quite straightforward.

    1. Acceleration measurements, stored in the variable *accels_B*, are converted to
       floating-point values and normalized using the function *VectorNormalize()*.  Results are
       stored in the variable *aHat_B*.
       
    2. The normalized gravity vector is formulated from the acceleration measurement by negating
       the acceleration measurements.
       
    3. The components of the gravity unit-vector are used to compute the roll and pitch angles,
       which are stored in the structure *gLeveler*.
       
The relevant code snippet is found below.

.. note::

    ‘_B’ denotes that the measurement is coordinatized in the body-frame while ‘_BinN’ indicates
    that the computed attitude is of the body (B) relative to the local inertial-frame (N).

::

    // Compute the acceleration unit-vector
    accelsFloat_B[X_AXIS] = (float)accels_B[X_AXIS];
    accelsFloat_B[Y_AXIS] = (float)accels_B[Y_AXIS];
    accelsFloat_B[Z_AXIS] = (float)accels_B[Z_AXIS];
    
    VectorNormalize( accelsFloat_B, aHat_B );
    
    // Form the gravity vector in the body-frame (negative of the
    //   accelerometer measurement)
    gHat_B[X_AXIS] = -aHat_B[X_AXIS];
    gHat_B[Y_AXIS] = -aHat_B[Y_AXIS];
    gHat_B[Z_AXIS] = -aHat_B[Z_AXIS];
    
    // Form the roll and pitch angles (in radians) from the gravity
    //   unit-vector.  Formulation is based on a 321-rotation sequence
    //   and assumption that the gravity-vector is constant.
    gLeveler.measuredEulerAngles_BinN[ROLL]  = (real)( atan2( gHat_B[Y_AXIS],
                                                              gHat_B[Z_AXIS] ) );
    gLeveler.measuredEulerAngles_BinN[PITCH] = (real)( -asin( gHat_B[X_AXIS] ) );


In addition to the algorithm, a timer, *timerCntr*, was added to the algorithm structure that
increments at the calling frequency of the leveler algorithm. The full algorithm, along with
initialization and supporting code, is found in *UserAlgorithm.c*.









