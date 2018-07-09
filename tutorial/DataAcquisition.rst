Data Acquisition
*****************

.. contents:: Contents
    :local:

OpenIMU makes data-acquision simple by reducing the steps required to get high-quality, inertial
sensor data.  Sensor drivers and calibration steps are handled without the need for additional
effort.

*TaskDataAcqusition* calls the routines that acquire sensor measurements, filters the data, and applies
calibration.  After these steps are complete, the task calls the following function:

::

    inertialAndPositionDataProcessing(dacqRate);


which provides functions to acquire sensor data for use by the user.


Acquiring Sensor Data
======================

Inside *inertialAndPositionDataProcessing()* several getter-functions, which obtain sensor data,
are provided.  Function names, which are described in the following table, were selected to
explicitly state their tasks:


+-----------------------------+---------------------------+-----------------------------------+
|                             |                           |                                   |
|  **Getter Function**        | **Description**           | **Units**                         |
|                             |                           |                                   |
+=============================+===========================+===================================+
|                             |                           |                                   |
| *GetAccelData_g()*          |                           | :math:`[g]`                       |        
|                             |                           |                                   |
+-----------------------------+ Obtain accelerometer data +-----------------------------------+
|                             |                           |                                   |
| *GetAccelData_mPerSecSq()*  |                           | :math:`[{m \over s^2}]`           |                             
|                             |                           |                                   |
+-----------------------------+---------------------------+-----------------------------------+
|                             |                           |                                   |
| *GetRateData_radPerSec()*   |                           | :math:`[{r \over s}]`             |               
|                             |                           |                                   |
+-----------------------------+ Obtain rate-sensor data   +-----------------------------------+
|                             |                           |                                   |
| *GetRateData_degPerSec()*   |                           | :math:`[{° \over s}]`             |               
|                             |                           |                                   |
+-----------------------------+---------------------------+-----------------------------------+
|                             |                           |                                   |
| *GetMagData_G()*            | Obtain magnetometer data  | :math:`[G]`                       |               
|                             |                           |                                   |
+-----------------------------+---------------------------+-----------------------------------+
|                             |                           |                                   |
| *GetBoardTempData()*        | Obtain temperature data   | :math:`[°C]`                      |
|                             |                           |                                   |
+-----------------------------+---------------------------+-----------------------------------+


Most inertial algorithm development will use :math:`[{m \over s^2}]`, :math:`[{r \over s}]`, and
:math:`[G]`, however :math:`[g]` and :math:`[{° \over s}]` are also available for the designer who
chooses to use data with these units.

These getters populate the arrays *accels*, *rates*, and *mags* then pass the arrays to the
function *RunUserNavAlgorithm()*.  This function is left as a placeholder inside which algorithm
development takes place.

       
Filtering Sensor Data
======================

**Under development ...**


Obtaining Sensor Data
======================

The arguments to *RunUserNavAlgorithm()* consist of pointers to the inertial sensor data as well as
a pointer the the GPS data structure (still under development).  Within the function, sensor data
can be quickly accessed by referencing the particular element of the sensor data-array of interest.
For instance, to obtain y-axis acceleration data, simply reference the second-element of the
acceleration-array, as follows:

::

    g_B[Y_AXIS] = -accels_B[Y_AXIS];




