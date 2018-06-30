Coordinate Frames
==================

.. contents:: Contents
    :local:

.. role::  raw-html(raw)
    :format: html


A body's position and orientation can only be measured relative to another coordinate-frame (set of basis vectors).  In this
formulation, inertial sensors provide the information to compute the attitude and position of a body in space relative to an
"inertial" frame, such as the Earth-Centered, Earth-Fixed frame (ECEF) or the North/East/Down-frame (NED)\ [#inertial]_.  The
equations to come use the superscripts listed in Table 1 to specify the frame in which a variable is measured.

**Table 1: Frames and their identifiers used throughout Algorithm Derivation**


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

*Figure 1* shows the relative orientation of three of the four frames listed in *Table 1* (ECEF not shown) for a hypothetical
body on the surface of the Earth with a roll of 20°, a pitch of 10°, and a heading of 30°.  The dashed red lines illustrate the
components of the ⊥-frame axes in the N-Frame while the dashed blue lines indicate the projection of the B-Frame axes onto the
N-frame.

|CoordFrames|

**Figure 1: Coordinate Frames used in Derivation (N, ⊥, and B-Frames)**


.. |xSubPerp| replace:: x\ :sub:`⊥`
.. |ySubPerp| replace:: y\ :sub:`⊥`
.. |zSubPerp| replace:: z\ :sub:`⊥`
.. |xSubB| replace:: x\ :sub:`⊥`
.. |ySubB| replace:: y\ :sub:`⊥`
.. |zSubB| replace:: z\ :sub:`⊥`

.. [#inertial] Strictly speaking, neither the ECEF-frame nor the NED-frame are inertial.  Both move and rotate relative to the
               stars; the NED-frame changes with location as well.  However, the two are sufficient for use with the OpenIMU
               line of products.

.. |CoordFrames| image:: ../media/CoordFrames.png
   :width: 5.0in
