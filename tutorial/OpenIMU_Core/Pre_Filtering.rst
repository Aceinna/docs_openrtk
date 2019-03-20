***********************************************
Default Pre-Filtering and Calibration Functions
***********************************************

.. contents:: Contents
    :local:
    
.. toctree::
    :maxdepth: 1
    
.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>

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
       *UserConfigurationStruct*.  This is the approach taken in this example.
    2. The configuration can be changed (either temporarily or permanently) using the Aceinna
       Navigation Studio interface.
    3. Commands can be sent to the unit over the serial interface.  This enables the cutoff
       frequency to be changed during operation, if desired.


**Calibration**:

Once filtered, the OpenIMU firmware then applies calibration data to the sensor counts,
compensating for temperature-related bias effects, sensor scale-factors, and misalignment.


