********************
Data Acquisition
********************

.. contents:: Contents
    :local:

OpenIMU makes data-acquisition simple by reducing the steps required to obtain high-quality,
inertial sensor data. Sensor drivers, signal filtering, and application of the calibration to the
raw measurements are handled without the need for additional user input.


The main routine controlling sensor sampling and processing is *TaskDataAcqusition*.  This task
calls the routines that acquire sensor measurements, filter the data, and apply calibration. In
particular, the task calls the following:

::

    inertialAndPositionDataProcessing(dacqRate);


*inertialAndPositionDataProcessing()* provides routines that acquire sensor data, call the
user-developed algorithm, and (if needed) write the results into an output stream.


Filtering and Calibration of Sensor Data
=========================================

The OpenIMU platform samples accelerometer and rate-sensor data at 800 Hz.  This data is then
filtered using a digital 800 Hz low-pass Butterworth filter with a 50 Hz cutoff frequency.  This
step is done to mitigate aliasing issues related to down-sampling of the data.  After this,
additional filtering (if desired) can be performed.  Finally, calibration is applied to the sensor
data.


**Filtering**:

Several built-in digital filters are available to the user to provide additional filtering.  In
particular, a selection of second-order Butterworth low-pass filters are provided.  Butterworth
filters were chosen for their maximally flat passband and straight-forward frequency responses.
Available cutoff-frequencies are:

    * 50 Hz
    * 40 Hz
    * 25 Hz
    * 20 Hz
    * 10 Hz
    * 5 Hz
    * 2 Hz
    * 0 Hz (Unfiltered)


In the firmware, these filter are implemented using fixed-point math (which operate on sensor
counts, not floating-point values).  This was done done to take advantage of the speed associated
with integer-math operations.


Built-in filters are selected in several different ways:

    1. Cutoff frequencies can be set in the default user-configuration structure,
       *UserConfigurationStruct*.
    2. The configuration can be changed (either temporarily or permanently) using the Aceinna
       Navigation Studio interface.
    3. Commands can be sent to the unit over the serial interface.  This enables the cutoff
       frequency to be changed during operation, if desired.


**Calibration**:

Once filtered, the OpenIMU firmware then applies calibration data to the sensor counts,
compensating for temperature-related bias effects, sensor scale-factors, and misalignment.


Acquiring Sensor Data
======================

Inside *inertialAndPositionDataProcessing()* several getter-functions are provided.  These functions
obtain sensor data directly from the sensor data-buffers.  Function names, described in the following
table, were chosen to make the task of each function clear.

.. table:: **Table 1: Sensor Measurement Getter Functions**

    +-----------------------------+---------------------------+----------------------+
    |                             |                           |                      |
    |  **Getter Function**        | **Description**           | **Units**            |
    |                             |                           |                      |
    +=============================+===========================+======================+
    |                             |                           |                      |
    | *GetAccelData_g()*          |                           | :math:`[g]`          |
    |                             |                           |                      |
    +-----------------------------+ Obtain accelerometer data +----------------------+
    |                             |                           |                      |
    | *GetAccelData_mPerSecSq()*  |                           | :math:`[{m / s^2}]`  |
    |                             |                           |                      |
    +-----------------------------+---------------------------+----------------------+
    |                             |                           |                      |
    | *GetRateData_radPerSec()*   |                           | :math:`[{r / s}]`    |
    |                             |                           |                      |
    +-----------------------------+ Obtain rate-sensor data   +----------------------+
    |                             |                           |                      |
    | *GetRateData_degPerSec()*   |                           | :math:`[{° / s}]`    |
    |                             |                           |                      |
    +-----------------------------+---------------------------+----------------------+
    |                             |                           |                      |
    | *GetMagData_G()*            | Obtain magnetometer data  | :math:`[G]`          |
    |                             |                           |                      |
    +-----------------------------+---------------------------+----------------------+
    |                             |                           |                      |
    | *GetBoardTempData()*        | Obtain temperature data   | :math:`[°C]`         |
    |                             |                           |                      |
    +-----------------------------+---------------------------+----------------------+


.. note::

    Most inertial algorithm development will use :math:`[{m / s^2}]`, :math:`[{r / s}]`, and
    :math:`[G]`.  However getters that provide accelerometer and rate-sensor data in :math:`[g]`
    and :math:`[{° / s}]` are also available for the designer who chooses to work in these units.

These getters populate the IMU data structure arrays *gIMU.accel_g*, *gIMU.rate_radPerSec*, and
*gIMU.mag_G*, defined in *UserMessaging.h*, with the associated sensor data.

::

    // IMU data structure
    typedef struct {
        // Timer output counter
        uint32_t timerCntr, dTimerCntr;
    
        // Algorithm states
        double accel_g[3];
        double rate_radPerSec[3];
        double mag_G[3];
        double temp_C;
    } IMUDataStruct;
    
    extern IMUDataStruct gIMU;


Passing Sensor Data to the Algorithm
=====================================

*RunUserNavAlgorithm()* is left as a placeholder inside which algorithm development takes place.
Arguments to the function consist of pointers to the inertial sensor data as well as a pointer to
the GPS data structure (still under development).


.. note::

    As GPS data is unused in this application, a *NULL pointer* is passed to the function in lieu
    of the GPS data-structure.  If the application requires GPS data, and it is available, then
    the pointer to the GPS data-structure should be passed instead.


::

    results = RunUserNavAlgorithm(gIMU.accel_g, gIMU.rate_radPerSec, gIMU.mag_G, NULL, dacqRate);


Within the function, sensor data can be easily accessed by referencing the particular element of
the sensor data-array of interest.  For instance, to obtain y-axis acceleration data, simply
reference the second-element of the acceleration-array, as follows:


::

    g_B[Y_AXIS] = -accels[Y_AXIS];


.. note::

    Identifiers such as X_AXIS, Y_AXIS, ROLL, PITCH, etc. are defined in *Indices.h*.  These are
    used in the firmware to make complex algorithms easier to understand.

