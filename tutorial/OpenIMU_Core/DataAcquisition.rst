**********************************
Default Data Acquisition Functions
**********************************

.. contents:: Contents
    :local:
    
.. toctree::
    :maxdepth: 1
    
.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


OpenIMU makes data-acquisition simple by reducing the steps required to get high-quality, inertial
sensor data.  Sensor drivers, filtering, and calibration are handled without the need for
additional user input.

The main routine controlling sensor sampling and processing is *TaskDataAcquisition*.  This task
calls the routines that acquire sensor measurements, filter the data, and apply calibration.  In
particular, the task calls the following, which provides functions to acquire sensor data:

::

    inertialAndPositionDataProcessing(dacqRate);


After completion of the sensor processing steps, it then calls the algorithm that operates on
sensor readings to create processed output.  


Acquiring Sensor Data
======================

Inside *inertialAndPositionDataProcessing()* several getter-functions are provided.  These functions
obtain sensor data directly from the sensor data-buffers.  Function names, described in the following
table, were chosen to make the task of each function clear.

.. table:: **Sensor Measurement Getter Functions**

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
    | *GetRateData_degPerSec()*   |                           | [°/s]                |               
    |                             |                           |                      |
    +-----------------------------+---------------------------+----------------------+
    |                             |                           |                      |
    | *GetMagData_G()*            | Obtain magnetometer data  | :math:`[G]`          |               
    |                             |                           |                      |
    +-----------------------------+---------------------------+----------------------+
    |                             |                           |                      |
    | *GetBoardTempData()*        | Obtain temperature data   | [°C]                 |
    |                             |                           |                      |
    +-----------------------------+---------------------------+----------------------+


.. note::

    Most inertial algorithm development will use :math:`[{m / s^2}]`, :math:`[{r / s}]`, and
    :math:`[G]`.  However getters that provide accelerometer and rate-sensor data in :math:`[g]`
    and [°/s] are also available for the designer who chooses to work in these units.


These getters work by populating the array whose address is provided as an argument to the
function.  In this example, the functions load the data directly into the data-structure elements
*gIMU.accel_g*, *gIMU.rate_degPerSec*, *gIMU.mag_G*, and *gIMU.temp_C*.

.. note::

    Structure elements (*accel_g*, *rate_degPerSec*, etc.) are all defined as doubles in the data
    structure created in *UserMessaging.h*.  This is done to match the datatype required by the
    getter functions, described above.


::

    // IMU data structure
    typedef struct {
        // Timer output counter
        uint32_t timerCntr, dTimerCntr;
    
        // Algorithm states
        double accel_g[3];
        double rate_degPerSec[3];
        double mag_G[3];
        double temp_C;
    } IMUDataStruct;
    
    extern IMUDataStruct gIMU;

