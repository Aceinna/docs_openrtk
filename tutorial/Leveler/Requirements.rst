********************
App Requirements
********************

.. contents:: Contents
    :local:
    
.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


Development of an application requires system level requirements that describe the purpose of the
application.  These requirements provide a guide for product development.  For the static-leveler
application, the following were selected:

    1. Measure roll and pitch angles of an object with a accuracy better than 0.10 [°]
    2. Operate in nominally quiescent conditions in which system acceleration levels are low
    3. Compute the attitude at a rate of 50 Hz
    4. Provide an output message consisting of the following:

       a. A relative time measurement (integer and decimal)
       b. Attitude consisting of roll and pitch angles in :math:`[°]`
       c. Acceleration readings in :math:`[{m / s^2}]`

