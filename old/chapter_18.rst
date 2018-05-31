Appendix G: Geodetic Coordinate Conversions
*******************************************

|image39|


|image40|


|image41|


|image42|


|image43|


|image44|


|image34|


.. [1]
   Note: certain combinations of baud-rate, packet-type, and output data
   rate are invalid because the time to transmit the data exceeds a
   limit on the permissible message length. The DMU381 limits the output
   packet width to 81% of the time between data packets. For instance,
   if the packet is output every 10 milliseconds (100 Hz) then the
   packet width must be less than 8 milliseconds or the combination is
   not allowed. This prevents messages from overlapping and causing
   communication problems. For this reason, 57.6 kbps and higher
   baud-rates are suggested.

.. [2]
   Register and data-packet availability is based on the features of the
   DMU381ZA (see `Table 2 <\l>`__).

.. [3]
   A SPI cycle consists of 16 clock cycles.

.. [4]
   A read-command consists of an 8-bit register address and a zero byte
   (0x00).

.. [5]
   Limits will affect the signal output only if the system is capable of
   generating a signal of that level. For instance, for an IMU381ZA-200,
   a 220 °/sec limit will apply however the 440 °/sec limit will not, as
   the sensor is incapable of outputting signals greater than 220 °/sec.

.. |image39| image:: media/appeng1.png
   :width: 5.66667in
   :height: 4.91042in
.. |image40| image:: media/appeng2.png
   :width: 5.66667in
   :height: 4.91042in
.. |image41| image:: media/appeng3.png
   :width: 5.66667in
   :height: 4.91042in
.. |image42| image:: media/appeng4.png
   :width: 5.66667in
   :height: 4.91042in
.. |image43| image:: media/appeng5.png
   :width: 5.66667in
   :height: 4.91042in
.. |image44| image:: media/appeng6.png
   :width: 5.66667in
   :height: 4.91042in
.. |image34| image:: media/image13.png
   :width: 1.66667in
   :height: 0.91042in
