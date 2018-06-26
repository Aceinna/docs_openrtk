Attitude Parameters
==================

.. contents:: Contents
    :local:

.. role::  raw-html(raw)
    :format: html

A body's position and orientation can only be measured relative to another coordinate-frame (set of basis vectors).  In this formulation, inertial sensors provide the information to compute the attitude and position of a body in space relative to an "inertial" frame, such as the Earth-Centered, Earth-Fixed frame (ECEF) or the North/East/Down-frame (NED) .  The equations to come use the superscripts listed in Table 1 to specify the frame in which a variable is measured.

.. |Perp| replace:: :raw-html:`&perp;`
.. |Perp2| replace:: :raw-html:`&perp;`
.. |H2O| replace:: H\ :sub:`&perp;`\ O
.. |xSubPerp| replace:: x\ :sub:`⊥`
.. |ySubPerp| replace:: y\ :sub:`⊥`
.. |zSubPerp| replace:: z\ :sub:`⊥`
.. |xSubB| replace:: x\ :sub:`⊥`
.. |ySubB| replace:: y\ :sub:`⊥`
.. |zSubB| replace:: z\ :sub:`⊥`

+-------------+-----------------+-----------------------------------------------------------------------------------+
|  **Frame**  | **Superscript** |  **Description**                                                                  |
+=============+=================+===================================================================================+
| ECEF-Frame  | E               || Frame aligned with Earth's axis (z-axis parallel to axis-of-                     |
|             |                 || rotation, x-axis exits at the equator through the prime-                         |
|             |                 || meridian); rotates with the Earth                                                |
+-------------+-----------------+-----------------------------------------------------------------------------------+
| NED-Frame   | N               || Frame aligned with the local tangent-frame (z-axis parallel to                   |
|             |                 || the gravity vector) with the x-axis aligned with true or                         |
|             |                 || magnetic north.  Red lines in *Figure 1*.                                        |
+-------------+-----------------+-----------------------------------------------------------------------------------+
| Perp-Frame  | P               || Frame aligned with the local tangent-frame (|zSubPerp|\ -axis parallel to        |
|             |                 || the gravity vector).  Dark blue lines in *Figure 1*                              |
+-------------+-----------------+-----------------------------------------------------------------------------------+
| Body-Frame  | B               || Frame aligned with the body-frame.  |xSubB|\ -axis lies in the plane             |
|             |                 || formed by the |xSubPerp| and |zSubPerp|\ -axes.  Light blues lines in *Figure 1* |
+-------------+-----------------+-----------------------------------------------------------------------------------+

*Figure 1* shows the relative orientation of three of the four frames listed in *Table 1* (ECEF not shown) for a hypothetical body on the surface of the Earth with a roll of 20\ :raw-html:`&deg;`, a pitch of 10\ :raw-html:`&deg;`, and a heading of 30\ :raw-html:`&deg;`.  The dashed red lines illustrate the components of the |Perp|\ -frame axes in the N-Frame while the dashed blue lines indicate the projection of the B-Frame axes onto the N-frame.


|CoordFrames|



 :raw-html:`&perp;`
  plane  |

|             |                 || formed by the x :sub: :raw-html:`&perp;`  and z                |
|             |                 || :sub: :raw-html:`&perp;`   |
|             |                 || -axes.  Light blue lines in *Figure 1*.                        |



The difference between t\ :sub:`ground` and t\ :sub:`sky` gives us a
reading known as t\ :sub:`⊥`\ .



|Perp|




|Perp2|

The difference between t\ :sub:`ground` and t\ :sub:`sky` gives us a
reading known as t\ :sub:`?`\ .

The power series, :math:`\frac{1}{0!}+\frac{2}{1!}x+\frac{3}{2!}x^2+\frac{4}{3!}x^3+...` states
that...



The well known quadratic formula:

.. math::


    x = -b \pm \frac{\sqrt{b^{2}-4ac}}{2a}

 
   
The |biohazard| symbol must be used on containers used to dispose
of medical waste.

The :sub:|biohazard| symbol must be used on containers used to dispose
of medical waste.


The `biohazard`:sub: symbol...




|7430-0131_fig_15|

The `Perp`:sub: symbol...

The :raw-html:`&perp;`:sub: symbol...




And then you can write:

This way :raw-html:`&rarr;`


Degree :raw-html:`&deg;`


.. |CoordFrames| image:: ../media/CoordFrames.png
   :width: 10.0in



.. |biohazard| image:: ../media/attention.png
   :width: 0.24in
   :height: 0.85in
   
   