Obtaining Sensor Data
**********************

.. contents:: Contents
    :local:


The arguments to *RunUserNavAlgorithm()* consist of pointers to the inertial sensor data as well as
a pointer the the GPS data structure (still under development).  Within the function, sensor data
can be quickly accessed by referencing the particular element of the sensor data-array of interest.
For instance, to obtain y-axis acceleration data, simply reference the second-element of the
acceleration-array, as follows:

::

    g_B[Y_AXIS] = -accels_B[Y_AXIS];


