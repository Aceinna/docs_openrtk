Time system
===========

OpenRTK internally uses GPST (GPS Time) for GNSS data handling and positioning algorithms. 
The time of input data expressed in other time systems like UTC  (Universal Time Coordinated) 
is converted to GPST before internal processing or the GPST of the internal data is converted 
to the appropriate other time system before output. One of the reasons why using GPST is to avoid 
leap seconds handling. UTC, which is the most generally used time system, is not a continuous 
time system with a second jump by the leap second insertion or deletion. 

The GPST is often expressed as a GPS week number and TOW (time of week) in seconds 
since the start epoch of 00:00:00 UTC on January 6, 1980. RTKLIB, however, does not 
use such a convention. In GNSS data processing, we often need to convert a time to 
a range or a range to a time. The TOW even expressed as a double precision has only the 
resolution of :math:`1.3 \times 10^{-10}` s in time, which is equivalent to the resolution of 0.04 m in range.

GPS Time and Universal Time Coordinated
---------------------------------------

The rough conversion of GPST to UTC (Universal Time Coordinated) or UTC to GPST can be 
expressed simply as:

.. math::

  t_{UTC} = t_{GPS} - \Delta t_{LS}

  t_{GPS} = t_{UTC} + \Delta t_{LS}


where :math:`t_{UTC}` and :math:`t_{GPS}` are the time expressed in UTC (s) and the time in GPST (s). 
:math:`\Delta t_{LS}` is the delta time (s) between UTC and GPST due to the cumulative leap seconds since January 6, 1980. 

The accuracy of the approximation in formula above is within several 10 ns. By using the UTC parameters in GPS navigation messages, 
we can convert GPST to UTC or UTC to GPST more accurately as:

.. math::

  t_{UTC} = t_{GPS} - {\Delta t_{LS} + A_0 + A_1(t_E - t_{ot} + 604800(WN - WN_t))}

where :math:`A_0, A_1, t_E, t_{ot}, WN are WN_t` are the UTC parameters provided in GPS navigation messages. 
More strictly, UTC in formula above is UTC(USNO), which is the US local implementation of UTC. 
The difference between UTC and UTC(USNO) can be obtained in Circular T provided by BIPM [72]. 
The difference is usually several ns level. 


GLONASS Time
------------

GLONASST (GLONASS Time) is based on UTC(SU) and includes leap second insertion or deletion. 
GLONASST is also aligned to the local time. So, roughly, the time :math:`t_{GLONASS}` (s) in GLONASST 
can be converted to the time :math:`t_{UTC}` (s) in UTC.

.. math::

  t_{UTC} = t_{GLONASS} - 10800

More accurately, the UTC parameters for GLONASST in GLONASS navigation message should be 
used similar to the GPST and UTC conversion. Ignoring the leap seconds and the 3 hour offset, 
the difference between GPST and GLONASST is usually 100 or several 100 ns level. 

Galileo Time
------------

GST (Galileo System Time) is composed of week number from the origin of the Galileo time 
and the TOW (time of week) in seconds. The GST start epoch is 00:00:00 UTC on August 22, 
1999. At the start epoch, GST shall be ahead of UTC by 13 seconds. The GST is continuous 
time without leap second insertion or deletion. So, the GST is aligned to GPST except for 
the 1024 weeks difference of the time system origin and a small time offset (GGTO). Note that 
the Galileo week number is provided as equal to the GPS week number in the RINEX convention. 

BDS Time
--------

BDT (BeiDou Navigation Satellite System Time) is a continuous time system without 
leap second insertion or deletion. The start epoch of BDT is 00:00:00 UTC on 
January 1, 2006. The offset of BDT with respect to UTC is controlled within 100 ns 
(modulo 1 second). So, the time :math:`t_{GPS}` (s) in GPST can roughly be converted to the 
time :math:`t_{BDT}` (s) in BDT within the accuracy of 200 ns as: 

.. math::

  t_{BDT} = t_{GPST} - 14

More accurately, the UTC parameters for BDT in BeiDou navigation messages 
should be used similar to the GPST and UTC conversion. 



