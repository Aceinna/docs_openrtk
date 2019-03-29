J1939 Data Acquisition Task
---------------------------

**src/taskDataAcquisition.c**.  This function implements the FreeRTOS Data Acquisition task.

    *Initialization*.  A task initialization block which is executed once, then the function enters an event loop:

    *Event loop*

    *   User Commands are received and processed, which potentially requires user messages to be build and sent.
    *   Packets that the user has selected for periodic transmission are built and sent to the user over the UART channel.
    *   Prepare new Data Acquisition Tick.  The timer that is used by the Data Acquisition Task is incremented,
        which prepares the task for the next data acquisition tick.
    *   Hardware BIT is performed.
    *   The task blocks until the the *Data Acquisition Ready* (dataAcqSem) semaphore is available, at which time the
        remainder of the event loop is activated.
    *   Data is acquired from the primary sensors (accelerometer and rate-sensor)
        as fast as possible.
    *   Data is acquired from the magnetometer (if present) at a lower rate.
    *   Data values that are out of the allowed range are adjusted to be at the nearest limit of the
        allowed range.
    *   The hardware BIT data, software BIT data, and overall status data are updated.
    *   The sensor data is processed by the function *inertialAndPositionDataProcessing* in
        *src/user/dataProcessingAndPresentation.c*.
    *   The user algorithm further processes the data.
    *   If the unit has a magnetometer, the magnetometer data is used to align the Euler angles and magnetic
        vector.
    *   The *CAN Data Ready* (canDataSem) semaphore is released so the *CAN Communication Task* can operate on the data.

**src/user/dataProcessingAndPresentation.c**.  This file provides a function to initialize the user algorithm and the function *inertialAndPositionDataProcessing* called from the Data Acquisition task to processes the sensor data.

**src/user/UserAlgorithm.c**  This file provides the functions that perform the user algorithm.  In this casethe full EKF algorithm is performed.   Also, this example application is configured to use the VG or AHRS algorithm, but it is not configured to use any of the other example algorithms.

**.piolibdeps/OpenIMU300-platform-library/Core/src/DataAcquisitionSupport.c**.  The 'dataAcqSem' is released in the Timer IRQ handler.  Release of the semaphore allows the Data Acquisition to take it and process data.
