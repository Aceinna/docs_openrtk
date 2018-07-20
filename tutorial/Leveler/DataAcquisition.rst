********************
Data Acquisition
********************

.. contents:: Contents
    :local:

OpenIMU makes data-acquision simple by reducing the steps required to get high-quality, inertial
sensor data. Sensor drivers, filtering, and calibration are handled without the need for additional
user input.


The main routine controlling sensor sampling and processing is TaskDataAcqusition. This task calls
the routines that acquire sensor measurements, filter the data, and apply calibration. In
particular, the task calls the following, which provides functions to acquire sensor data:

::

    inertialAndPositionDataProcessing(dacqRate);


After completion of the sensor processing steps, it then calls the algorithm that operates on
sensor readings to create processed output.  


Filtering and Calibration of Sensor Data
=========================================

The OpenIMU platform samples accelerometer and rate-sensor data at 800 Hz.  This data is then
filtered using a digital 800 Hz low-pass Butterworth filter with a 50 Hz cutoff frequency.  This
step is done to mitigate aliasing issues related to down-sampling of the data.  After this,
additional filtering (if desired) can be performed.  Finally, the calibration is applied to the
sensor data.


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

These getters populate the arrays *accels*, *rates*, and *mags*, defined in
*inertialAndPositionDataProcessing()*, with the associated sensor data.  These arrays are then
passed to the function *RunUserNavAlgorithm()*.  This function is left as a placeholder inside
which algorithm development takes place.


Obtaining Sensor Data
======================

Arguments to *RunUserNavAlgorithm()* consist of pointers to the inertial sensor data as well as a
pointer the the GPS data structure (still under development).  Within the function, sensor data can
be quickly accessed by referencing the particular element of the sensor data-array of interest.
For instance, to obtain y-axis acceleration data, simply reference the second-element of the
acceleration-array, as follows:

::

    g_B[Y_AXIS] = -accels_B[Y_AXIS];




