***************************************************************
Vertical-Gyro / Attitude and Heading Reference System Algorithm
***************************************************************

.. contents:: Contents
    :local:
    
.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


The mathematics behind the Vertical-Gyro / Attitude and Heading Reference System are based upon an
Extended Kalman Filter (EKF), which fuses the sensor readings into estimates for the attitude,
heading, and rate-sensor biases.  This algorithm is quite a bit more complicated than the
`Static-Leveler <../Leveler_App.html#static-leveler-application>`__, relying upon various
mathematical concepts such as linear-algebra, differential-equations, vector-calculus, and
stochastic systems (among other concepts) to formulate a solution.  These topics will not be
covered here but, for those interested, the derivation of the VG/AHRS algorithm is described in the
`EKF Algorithms <../../algorithms.html#ekf-algorithms>`__ section.
 

The VG implementation of the EKF differs only from the AHRS implementation in one respect: the AHRS
solution uses the magnetometer signal while the VG does not.  All other aspects are the same.


Implementation of the EKF algorithm in this example will consist of calling a pre-built algorithm
in *RunUserNavAlgorithm*.


Algorithm Development
=======================

VG/AHRS algorithm development consists of the following:

    1. Populating the EKF input structure
       
    2. Calling the EKF algorithm
       
    3. Populating the EKF output structure

    
These three steps are performed in *RunUserNavAlgorithm* found in *UserAlgorithm.c*.  The complete
sequence is below:

::

    void *RunUserNavAlgorithm(double *accels, double *rates, double *mags, gpsDataStruct_t *gps, int dacqRate)
    {
        // Populate the EKF input data structure.  Load the GPS data
        //   structure to NULL.
        EKF_SetInputStruct(accels, rates, mags, NULL);

        // Call the desired algorithm based on the EKF with different
        //   calling rates and different settings.
        _Algorithm(dacqRate, VG);

        // Fill the output data structure with the EKF states and other 
        //   desired information
        EKF_SetOutputStruct();

        // The returned value from this function is unused by external functions.  The
        //   NULL pointer is returned instead of a data structure.
        return NULL;
    }


.. note::

    *UserAlgorihm.c* contains additional code than what is shown above.  It is not shown here to
    highlight only the algorithm-related functions.


Populating EKF Input Structure
--------------------------------

The EKF input structure is part of the pre-built EKF algorithm and defined in *EKF_Algorithm.h*, as
follows:

::

    /* Global Algorithm structure  */
    typedef struct {
        // Sensor readings in the body-frame (B)
        double            accel_B[3];      // [g]
        double            angRate_B[3];    // [rad/s]
        double            magField_B[3];   // [G]
        
        // Used to generate the system ICs
        // GPS stuff still needed
    } EKF_InputDataStruct;

    extern EKF_InputDataStruct gEKFInputData; // 


The VG/AHRS algorithm operates only on inertial-sensor data to generate its estimates.  Sensor data
is obtained using the getter-functions in *inertialAndPositionDataProcessing*, populating the
sensor arrays *accels*, *rates*, and *mags*, which are passed into *RunUserNavAlgorithm*, as shown
above.  These arrays are then passed into *_PopulateEKFInputStruct()* to fill the EKF input
structure.

::

    // Populate the EKF input data structure.  Load the GPS data
    //   structure as NULL.
    EKF_SetInputStruct(accels, rates, mags, NULL);


Inside *EKF_SetInputStruct*, the data is placed into the appropriate structure fields:

::

    //Populate the EKF input structure with sensor and GPS data (if used)
    void EKF_SetInputStruct(double *accels, double *rates, double *mags, gpsDataStruct_t *gps)
    {
        // Accelerometer signal is in [g]
        gEKFInputData.accel_B[X_AXIS]    = accels[X_AXIS];
        gEKFInputData.accel_B[Y_AXIS]    = accels[Y_AXIS];
        gEKFInputData.accel_B[Z_AXIS]    = accels[Z_AXIS];
    
        // Angular-rate signal is in [rad/s]
        gEKFInputData.angRate_B[X_AXIS]  = rates[X_AXIS];
        gEKFInputData.angRate_B[Y_AXIS]  = rates[Y_AXIS];
        gEKFInputData.angRate_B[Z_AXIS]  = rates[Z_AXIS];
    
        // Magnetometer signal is in [G]
        gEKFInputData.magField_B[X_AXIS] = mags[X_AXIS];
        gEKFInputData.magField_B[Y_AXIS] = mags[Y_AXIS];
        gEKFInputData.magField_B[Z_AXIS] = mags[Z_AXIS];
    }

.. note::

    The VG/AHRS algorithm requires sensor data in :math:`[g]`, :math:`[{rad / s}]`,
    and :math:`[G]`.  Providing data in other units will result in an algorithm that does not
    operate properly.

    The suffix *_B* indicates the data is measured in the body-frame (B), the frame in which the
    sensors are located.  As multiple frames are used in the EKF algorithm, this notation is used
    to prevent confusion.


Calling the EKF Algorithm
--------------------------

After obtaining the required data, the algorithm can be called.  This is done in the function
*_Algorithm(dacqRate, algoType)*, where arguments to the function are the calling-frequency of the
data-acquisition task and the algorithm-type.  Algorithm-type is selected by specifying either *VG*
or *AHRS* as arguments.  These are defined in *GlobalConstants.h* as:

::

    // Algorithm specifiers
    #define  IMU   0
    #define  AHRS  1
    #define  VG    2
    #define  INS   3


Specifically, the VG algorithm is selected via the following algorithm function call:

::

    _Algorithm(dacqRate, VG);


while the AHRS algorithm is selected via the AHRS specifier:

::

    _Algorithm(dacqRate, AHRS);


As mentioned previously, the only difference between the VG and AHRS algorithm is that the AHRS
makes use of magnetometer data while the VG does not.  This behavior is handled internal to the
algorithm and controlled by the algorithm-behavior field, *gAlgorithm.Behavior*.  In particular,
the field *useMag* tells the algorithm what to do with magnetometer data.  In *_Algorithm* (shown
below), *useMag* bit is set in the algorithm initialization section, based on *algoType*:

::

    //
    void _Algorithm(int dacqRate, uint8_t algoType)
    {
        static int initAlgo = 1;
        static uint8_t algoCntr = 0, algoCntrLimit = 0;
    
        // Initialize the configuration variables needed to make the system
        //   generate a VG-type solution.
        if(initAlgo) {
            // Reset 'initAlgo' so this is not executed more than once.  This
            //   prevents the algorithm from being switched during run-time.
            initAlgo = 0;
    
            // Set the configuration variables for a VG-type solution
            //   (useMags = 0 forces the VG solution)
            gAlgorithm.Behavior.bit.freeIntegrate      = 0;
            gAlgorithm.Behavior.bit.useMag             = 0;
            gAlgorithm.Behavior.bit.useGPS             = 0;
            gAlgorithm.Behavior.bit.stationaryLockYaw  = 0;
            gAlgorithm.Behavior.bit.restartOnOverRange = 0;
            gAlgorithm.Behavior.bit.dynamicMotion      = 1;
    
            // While not needed, set hasMags to false
            enableMagInAlgorithm(FALSE);
    
            if(algoType == VG) {
                // Configuration already set for a VG solution
            } else if(algoType == AHRS) {
                // Set the configuration variables for AHRS solution
                //   (useMags = 1 and enable mags)
                enableMagInAlgorithm(TRUE);
                gAlgorithm.Behavior.bit.useMag = 1;
            } else if(algoType == INS) {
                while(1);
            } else {
                while(1);
            }
    
            algoCntr = 0;
            algoCntrLimit = (int)( dacqRate / (int)gAlgorithm.callingFreq );
            if( algoCntrLimit < 1 ) {
                // If this logic is reached, also need to adjust the algorithm
                //   parameters to match the modified calling freq (or stop the
                //   program to indicate that the user must adjust the program)
                algoCntrLimit = 1;
            }
        }
    
        // Aceinna VG/AHRS/INS algorithm
        if(algoCntr == 0) {
           EKF_Algorithm();
        }
    
        // Increment the counter.  If greater than or equal to the limit, reset
        //   the counter to cause the algorithm to run on the next pass through.
        algoCntr++;
        if(algoCntr >= algoCntrLimit) {
            algoCntr = 0;
        }
    }


**Algorithm Behavior Bits**

In addition to setting *useMag*, the initialization section sets other algorithm-behavior bits in
gAlgorithm.Behavior.  These are described in **Table 3**.


.. table:: **Table 3: Algorithm Behavior Bits**

    +----------------------+-----------------------------------------------------------------------------+
    |                      |                                                                             |
    | **bit**              | **Description**                                                             |
    |                      |                                                                             |
    +======================+=============================================================================+
    |                      || This bit controls the behavior of the propagation model in the Extended    |
    | *freeIntegrate*      || Kalman Filter.  When set true, the EKF stops performing updates for a      |
    |                      || certain time (nominally 60 seconds).  At the end of this period, the       |
    |                      || update functionality of the EKF is restored and the errors that built up   |
    |                      || over the free-integration period are removed.                              |
    |                      ||                                                                            |
    |                      || The default setting if false as this functionality cannot be commanded     |
    |                      || until the solution has converged.                                          |
    |                      ||                                                                            |
    |                      || This functionality is normally used when the user requires a good attitude |
    |                      || solution and knows short-term, extreme conditions are expected.            |
    +----------------------+-----------------------------------------------------------------------------+
    |                      || This bit instructs the algorithm to use magnetometer data to estimate      |
    | *useMag*             || a heading solution.  It does not enable the output of the magnetometer     |
    |                      || signal, which is continually being generated.                              |
    |                      ||                                                                            |
    |                      || When set true, an AHRS solution is generated and the magnetometer signal   |
    |                      || is used to estimate heading.  When false, the heading error computed by    |
    |                      || the EKF is set to zero.  In this case, no updates to heading based states  |
    |                      || are done (the z-axis rate-bias is not estimated and the heading error is   |
    |                      || not corrected).                                                            |
    +----------------------+-----------------------------------------------------------------------------+
    |                      || This bit tells the algorithm to use GPS information if available.  For the |
    | *useGPS*             || VG/AHRS algorithm, this (at present) has no effect but should be set       |
    |                      || false to prevent possible operational problems.                            |
    +----------------------+-----------------------------------------------------------------------------+
    |                      || This bit is only used as part of the INS solution.  For the VG/AHRS        |
    | *stationaryLockYaw*  || algorithm, this (at present) has no effect but should be set               |
    |                      || false to prevent possible operational problems.                            |
    +----------------------+-----------------------------------------------------------------------------+
    |                      || This bit instructs the algorithm to restart if any of the sensors exceed   |
    | *restartOnOverRange* || its operational limits.  This is nominally used for demonstration purposes |
    |                      || and should be set false to prevent the algorithm from restarting in during |
    |                      || operation.                                                                 |
    +----------------------+-----------------------------------------------------------------------------+
    |                      || The dynamic motion bit controls the transition from High-Gain mode (the    |
    | *dynamicMotion*      || initialization period of the algorithm) to Low-Gain (operational) mode.    |
    |                      || It should be set true to allow the transition to occur.  The bit can be    |
    |                      || set false to restart the algorithm in high-gain mode, if required.  But it |
    |                      || must be set high (by the user) to allow future transitions to occur.       |
    +----------------------+-----------------------------------------------------------------------------+


**Algorithm Calling Frequency**

In addition to setting algorithm behavior bits, the initialization routine sets the counter that
controls the calling-frequency of the algorithm.  It does this by calling the algorithm at some
fraction of the rate that the data-acquisition task is called.  Nominally this fraction is one and
the algorithm runs at the 200 Hz rate of the data-acquisition task.  However, this can be changed
to run the algorithm at a slower rate (say 100 Hz or 50 Hz) if the computational load does not
permit it to run at 200 Hz.


The calling rate of the algorithm is specified by *gAlgorithm.callingFreq*, which should be set
during algorithm initialization, *InitUserAlgorithm()*.  If this variable is set elsewhere in the
firmware, it must be followed by a call to *InitializeAlgorithmStruct()*, which sets other timing
variables based on *gAlgorithm.callingFreq*.  For example, this sequence of events occurs when the
algorithm-reset command is sent.


Populating EKF Output Structure
-------------------------------- 

Once the algorithm is executed, the EKF results are placed in the EKF output structure,
*gEKFOutputData*, defined in *EKF_Algorithm.c*.  This is done by the function
*EKF_SetOutputStruct()*, called in *RunUserNavAlgorithm*.


The complete function follows:

::

    void EKF_SetOutputStruct(void)
    {
        // ------------------ States ------------------

        // Position in [m]
        gEKFOutputData.position_N[0] = gKalmanFilter.Position_N[0];
        gEKFOutputData.position_N[1] = gKalmanFilter.Position_N[1];
        gEKFOutputData.position_N[2] = gKalmanFilter.Position_N[2];

        // Velocity in [m/s]
        gEKFOutputData.velocity_N[0] = gKalmanFilter.Velocity_N[0];
        gEKFOutputData.velocity_N[1] = gKalmanFilter.Velocity_N[1];
        gEKFOutputData.velocity_N[2] = gKalmanFilter.Velocity_N[2];

        // Position in [N/A]
        gEKFOutputData.quaternion_BinN[0] = gKalmanFilter.quaternion[0];
        gEKFOutputData.quaternion_BinN[1] = gKalmanFilter.quaternion[1];
        gEKFOutputData.quaternion_BinN[2] = gKalmanFilter.quaternion[2];
        gEKFOutputData.quaternion_BinN[3] = gKalmanFilter.quaternion[3];

        // Angular-rate bias in [deg/sec]
        gEKFOutputData.angRateBias_B[0] = gKalmanFilter.rateBias_B[0] * RAD_TO_DEG;
        gEKFOutputData.angRateBias_B[1] = gKalmanFilter.rateBias_B[1] * RAD_TO_DEG;
        gEKFOutputData.angRateBias_B[2] = gKalmanFilter.rateBias_B[2] * RAD_TO_DEG;

        // Acceleration-bias in [m/s^2]
        gEKFOutputData.accelBias_B[0] = gKalmanFilter.accelBias_B[0] * ACCEL_DUE_TO_GRAV;
        gEKFOutputData.accelBias_B[1] = gKalmanFilter.accelBias_B[1] * ACCEL_DUE_TO_GRAV;
        gEKFOutputData.accelBias_B[2] = gKalmanFilter.accelBias_B[2] * ACCEL_DUE_TO_GRAV;

        // ------------------ Derived variables ------------------

        // Euler-angles in [deg]
        gEKFOutputData.eulerAngs_BinN[0] = gKalmanFilter.eulerAngles[0] * RAD_TO_DEG;
        gEKFOutputData.eulerAngs_BinN[1] = gKalmanFilter.eulerAngles[1] * RAD_TO_DEG;
        gEKFOutputData.eulerAngs_BinN[2] = gKalmanFilter.eulerAngles[2] * RAD_TO_DEG;

        // Angular-rate in [deg/s]
        gEKFOutputData.corrAngRates_B[X_AXIS] = ( gEKFInputData.angRate_B[X_AXIS] -
                                                  gKalmanFilter.rateBias_B[X_AXIS] ) * RAD_TO_DEG;
        gEKFOutputData.corrAngRates_B[Y_AXIS] = ( gEKFInputData.angRate_B[Y_AXIS] -
                                                  gKalmanFilter.rateBias_B[Y_AXIS] ) * RAD_TO_DEG;
        gEKFOutputData.corrAngRates_B[Z_AXIS] = ( gEKFInputData.angRate_B[Z_AXIS] -
                                                  gKalmanFilter.rateBias_B[Z_AXIS] ) * RAD_TO_DEG;
        
        // Acceleration in [m/s^2]
        gEKFOutputData.corrAccel_B[X_AXIS] = ( gEKFInputData.accel_B[X_AXIS] -
                                               gKalmanFilter.accelBias_B[X_AXIS] ) * ACCEL_DUE_TO_GRAV;
        gEKFOutputData.corrAccel_B[Y_AXIS] = ( gEKFInputData.accel_B[Y_AXIS] -
                                               gKalmanFilter.accelBias_B[Y_AXIS] ) * ACCEL_DUE_TO_GRAV;
        gEKFOutputData.corrAccel_B[Z_AXIS] = ( gEKFInputData.accel_B[Z_AXIS] -
                                               gKalmanFilter.accelBias_B[Z_AXIS] ) * ACCEL_DUE_TO_GRAV;

        // ------------------ Algorithm flags ------------------
        gEKFOutputData.opMode         = gAlgorithm.state;
        gEKFOutputData.linAccelSwitch = gAlgorithm.linAccelSwitch;
        gEKFOutputData.turnSwitchFlag = gBitStatus.swStatus.bit.turnSwitch;
    }


These values can now be referenced by the serial messaging routines that create and populate output
messages.

