
********************
Attitude Parameters
********************

.. contents:: Contents
    :local:

.. role::  raw-html(raw)
    :format: html

.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>

This paper makes use of three different attitude parameters to specify the orientation of a body
(B) relative to another frame (such as the N-frame).

   #.  Direction Cosine Matrices
   #.  Quaternion Elements
   #.  Euler Angles


Direction Cosine Matrices
==========================

The first of these, the direction cosine matrix\ [#rot_BinN]_, |R_BinN|\ , specifies the attitude
of one frame relative to another by relaying how the basis-vectors of one frame relate to the
basis-vectors of another.  These matrices have the property that they can, in a straightforward
manner, transform vectors from one frame into another, such as from the Body to the NED-frame:

.. math::

    \vec{x}{^N} = {^N}{R}{^B} \cdot \vec{x}{^B}


In the upcoming derivation, transformations based on the Body-Fixed 3-2-1 Rotation set\ [#rot_321]_
and the formulation used by Thomas Kane\ [#Kane_Ref]_  are relied upon extensively.


Quaternion Elements
====================

The second parameter used to convey orientation information are quaternion elements\ [#quatElems]_
(also called Euler parameters), |q_BinN|.  Quaternions are relatively easy to propagate in time and
do not possess singularities.  However, they are not intuitive.  Quaternions consist of a scalar
and a vector component:


.. math::

    {^N}{\vec{q}}{^B} &= { \begin{bmatrix} {
                                            q_{0} \hspace{5mm} \vec{q}_{v}
                           } \end{bmatrix}
                         }^{T} \\
                      {\hspace{5mm}} \\
    &= { \begin{bmatrix} {q_{0} \hspace{5mm} q_{1} \hspace{5mm} q_{2} \hspace{5mm} q_{3}} \end{bmatrix} }^{T} \\
                      {\hspace{5mm}} \\
    &= { \begin{bmatrix} {
                           \cos{\begin{pmatrix} \theta \over 2 \end{pmatrix}} \hspace{5mm}
                           \hat{u} \cdot \sin{\begin{pmatrix} \theta \over 2 \end{pmatrix}}
         } \end{bmatrix}
       }^{T}


Euler Angles
=============

The final parameter used to relay attitude information are Euler angles.  These are more intuitive
than quaternions but, unlike quaternions, experience singularities at certain angles (based on the
selected rotation sequence).  For a 321-rotation sequence\ [#Rot_Seq_Usage]_, the singularity is at
a pitch of 90°.


Mathematical Relationships between Attitude Parameters
=======================================================

All three parameters contain the same information.  The equations that relate the various
parameters follow\ [#Quat_Ref]_.  For a 321-rotation sequence, the expression relating the rotation
transformation matrix of the body-frame in the NED-frame, |R_BinN| , to the quaternion elements,
|q_BinN|, is:

.. math::
    {{^N}{R}{^B}} = {
                      \begin{bmatrix} {
                                        \begin{array}{ccc} 
                                                           {{q_0}^2 + {q_1}^2 - {q_2}^2 - {q_3}^2} &
                                                           {2 \cdot { \begin{pmatrix} {q_1 \cdot q_2 - q_0 \cdot q_3} \end{pmatrix} }} &
                                                           {2 \cdot { \begin{pmatrix} {q_1 \cdot q_3 + q_0 \cdot q_2} \end{pmatrix} }}
                                                           \cr
                                                           {2 \cdot { \begin{pmatrix} {q_1 \cdot q_2 + q_0 \cdot q_3} \end{pmatrix} }} &
                                                           {{q_0}^2 - {q_1}^2 + {q_2}^2 - {q_3}^2} &
                                                           {2 \cdot { \begin{pmatrix} {q_2 \cdot q_3 - q_0 \cdot q_1} \end{pmatrix} }}
                                                           \cr
                                                           {2 \cdot { \begin{pmatrix} {q_1 \cdot q_3 - q_0 \cdot q_2} \end{pmatrix} }} &
                                                           {2 \cdot { \begin{pmatrix} {q_2 \cdot q_3 + q_0 \cdot q_1} \end{pmatrix} }} &
                                                           {{q_0}^2 - {q_1}^2 - {q_2}^2 + {q_3}^2}
                                        \end{array}
                      } \end{bmatrix}
                    }


|R_BinN| can also be expressed in terms of Euler-angles, :math:`{{^N}{\vec{\Theta}}{^B}} = { \begin{bmatrix} { {{^\perp}{\phi}{^B }} \hspace{5mm} {{^\perp}{\theta}{^B }} \hspace{5mm} {{^N}{\psi}{^\perp}} } \end{bmatrix} }^{T}`\ :


.. Comment --> Complete list of mathematical formatting commands found at http://www.onemathematicalcat.org/MathJaxDocumentation/TeXSyntax.htm#cr.

.. math::

    {{^N}{R}{^B}} = {
                      \begin{bmatrix} {
                                        \begin{array}{ccc} 
                                                           { \cos{\begin{pmatrix} {{^\perp}{\theta}{^B}} \end{pmatrix}} } &
                                                           { -\sin{\begin{pmatrix} {{^N}{\psi}{^\perp}} \end{pmatrix}} } &
                                                           { 0 }
                                                           \cr
                                                           { \sin{\begin{pmatrix} {{^N}{\psi}{^\perp}} \end{pmatrix}} } &
                                                           { \cos{\begin{pmatrix} {{^N}{\psi}{^\perp}} \end{pmatrix}} } &
                                                           {0}
                                                           \cr
                                                           {0} &
                                                           {0} &
                                                           {1}
                                        \end{array}
                      } \end{bmatrix}
                    }
                    \cdot
                    {
                      \begin{bmatrix} {
                                        \begin{array}{ccc} 
                                                           { \cos{\begin{pmatrix} {{^\perp}{\theta}{^B}} \end{pmatrix}} } &
                                                           { \sin{\begin{pmatrix} {{^\perp}{\theta}{^B}} \end{pmatrix}} \cdot \sin{\begin{pmatrix} {{^\perp}{\phi}{^B}} \end{pmatrix}} } &
                                                           { \sin{\begin{pmatrix} {{^\perp}{\theta}{^B}} \end{pmatrix}} \cdot \cos{\begin{pmatrix} {{^\perp}{\phi}{^B}} \end{pmatrix}} }
                                                           \cr
                                                           { 0 } &
                                                           { \cos{\begin{pmatrix} {{^\perp}{\phi}{^B}} \end{pmatrix}} } &
                                                           { -\sin{\begin{pmatrix} {{^\perp}{\phi}{^B}} \end{pmatrix}} }
                                                           \cr
                                                           { -\sin{\begin{pmatrix} {{^\perp}{\theta}{^B}} \end{pmatrix}} } &
                                                           { \cos{\begin{pmatrix} {{^\perp}{\theta}{^B}} \end{pmatrix}} \cdot \sin{\begin{pmatrix} {{^\perp}{\phi}{^B}} \end{pmatrix}} } &
                                                           { \cos{\begin{pmatrix} {{^\perp}{\theta}{^B}} \end{pmatrix}} \cdot \cos{\begin{pmatrix} {{^\perp}{\phi}{^B}} \end{pmatrix}} }
                                        \end{array}
                      } \end{bmatrix}
                    }


In this case, |R_BinN| is broken up into two sequential transformations, which separate the roll
and pitch calculations from the heading (this method is used later to form attitude measurements
from the sensor readings):


.. math::

	{{^N}{R}{^B}} = {{^N}{R}{^\perp}} \cdot {{^\perp}{R}{^B}}


Finally, Euler angles, |Theta_BinN|, can be expressed in terms of quaternion-elements, |q_BinN|:


.. math::

    {^\perp}{\phi}{^B}   &= {atan2}{ \begin{pmatrix} {
                                                   2 \cdot { \begin{pmatrix} {q_2 \cdot q_3 + q_0 \cdot q_1} \end{pmatrix} }, \hspace{2mm} {{q_0}^2 - {q_1}^2 - {q_2}^2 + {q_3}^2}
                                 } \end{pmatrix}
                               } \\
                      {\hspace{5mm}} \\
    {^\perp}{\theta}{^B} &= -{asin}{ \begin{pmatrix} {
                                                   2 \cdot { \begin{pmatrix} {q_1 \cdot q_3 - q_0 \cdot q_2} \end{pmatrix} }
                                 } \end{pmatrix}
                               } \\
                      {\hspace{5mm}} \\ 
    {^N}{\psi}{^\perp}   &= {atan2}{ \begin{pmatrix} {
                                                   2 \cdot { \begin{pmatrix} {q_1 \cdot q_2 + q_0 \cdot q_3} \end{pmatrix} }, \hspace{2mm} {{q_0}^2 + {q_1}^2 - {q_2}^2 - {q_3}^2}
                                 } \end{pmatrix}
                               } 


Note: due to the way the roll and pitch are separated from the heading, the Euler angles,
phi_Bin :math:`\perp`, theta_Bin :math:`\perp`, and psi :math:`_\perp` inN are the same if written as |phi_BinN|, |theta_BinN|, and
|psi_BinN|.


Example
========

Using the direction cosine matrix formulation, the transformation to get from the body to inertial-
frame (ECEF) is composed of multiple transformations (*Figure*):

   #.  Transformation from the (light-blue) body-frame to the (dark blue) local perpendicular-frame
       :math:`(\perp), R_Bin \perp`
   #.  Transformation from the (dark blue) :math:`\perp`-frame to the (red) local NED-frame, R :math:`_\perp` inN
   #.  Transformation from the (red) NED-frame to the ECEF-frame, |R_NinE| (ECEF-Frame not shown;
       black line are latitude and longitude lines).  |R_NinE| is based on the WGS84 model.


This notation not only makes the formulation easier by simplifying the full complexity of the
transformation but it helps avoid confusion by explicitly specifying the frame used in each
calculation:

.. math::

    {^E}{R}{^B} = {^E}{R}{^N} \cdot {^N}{R}{^\perp} \cdot {^\perp}{R}{^B}


Some additional information about these frames:

   #.  |R_NinE|, the transformation between the NED and Earth-frame (used in the INS formulation),
       is solely a function of ECEF location, :math:`{^E}{R}{^N} = f({\vec{r}}{^E})`\ , and is
       based on the WGS84 model.
   #.  |R_BinN|, the transformation between the NED and body-frame is solely a function of the
       attitude of the body-frame (roll, pitch, and heading angles of the body) and can be measured
       by the local gravity and magnetic-field vectors (or GPS heading),
       :math:`{^N}{R}{^B} = f({\vec{g}}, {\vec{b}})`



.. |Perp| replace:: :raw-html:`&perp;`
.. |Perp2| replace:: :raw-html:`&perp;`
.. |H2O| replace:: H\ :sub:`&perp;`\ O
.. |xSubPerp| replace:: x\ :sub:`\perp`
.. |ySubPerp| replace:: y\ :sub:`\perp`
.. |zSubPerp| replace:: z\ :sub:`\perp`
.. |xSubB| replace:: x\ :sub:`\perp`
.. |ySubB| replace:: y\ :sub:`\perp`
.. |zSubB| replace:: z\ :sub:`\perp`




.. |R_BinN| replace:: :math:`{^N}{R}{^B}`
.. |q_BinN| replace:: :math:`{^N}{\vec{q}}{^B}`

.. |R_LinN| replace:: :math:`{^N}{R}{^L}`

.. |RSub321| replace:: :math:`{R}_{321}`

.. |Theta_BinN| replace:: :math:`{^N}{\vec{\Theta}}{^B}`

.. |phi_Bin\perp| replace:: :math:`{^\perp}{\phi}{^B}`
.. |theta_Bin\perp| replace:: :math:`{^\perp}{\theta}{^B}`
.. |psi_\perpinN| replace:: :math:`{^N}{\psi}{^\perp}`

.. |phi_BinN| replace:: :math:`{^N}{\phi}{^B}`
.. |theta_BinN| replace:: :math:`{^N}{\theta}{^B}`
.. |psi_BinN| replace:: :math:`{^N}{\psi}{^B}`

.. |R_Bin\perp| replace:: :math:`{^\perp}{R}{^B}`
.. |R_\perpinN| replace:: :math:`{^N}{R}{^\perp}`
.. |R_NinE|  replace:: :math:`{^E}{R}{^N}`

.. [#rot_BinN] Pronounced “R B-in-N” and refers to the orientation of the B-Frame in the N-Frame.
               Also referred to as a rotation transformation matrix.

.. [#rot_321] A 3-2-1 rotation set defines the attitude of one set of basis-vectors (local-frame)
              relative to another by specifying the angles of rotation required to get from the
              inertial (N) to the local-frame (L).  With the local and inertial-frames initially
              aligned, the rotations are performed in the following order: the first is about the
              local z-axis (3), followed by a rotation about the local y-axis (2), and finally by a
              rotation about the local x-axis (1).  The resulting matrix, |R_LinN| = |RSub321|, is
              composed of column vectors formed from the xyz-axes of the local-frame coordinatized
              in the inertial-frame: 
              |R_LinN| = :math:`\begin{bmatrix} {{{\hat{x}_{L}}{^N}} \hspace{5mm} {{\hat{y}_{L}}{^N}} \hspace{5mm} {{\hat{z}_{L}}{^N}}} \end{bmatrix}`\ .


.. [#Kane_Ref] Kane, Thomas R.; Levinson, David A. (1985), Dynamics, Theory and Applications,
               McGraw-Hill series in mechanical engineering, McGraw Hill.  Note: one main
               difference between Kane’s approach is the DCM is the transpose of the DCM of other
               formulations; I think Kane’s formulation is more intuitive.


.. [#quatElems] Commonly referred to simply as “quaternion”.  To make it easier to reference the
                elements in c, c++, and python, the first quaternion-element (the scalar component
                of the quaternion) will have the zero index and is expressed as
                :math:`{q}_{0}=\cos \begin{pmatrix} \theta / 2 \end{pmatrix}`.  The vector
                component of the quaternion,
                :math:`{\vec{q}}_{v}=\hat{u} \cdot \sin \begin{pmatrix} \theta / 2 \end{pmatrix}`,
                occupies elements 2, 3, and 4.


.. [#Rot_Seq_Usage] The 321-rotation sequence is the only rotation sequence considered in this
                    paper.


.. [#Quat_Ref] Based on unpublished notes by Keith Reckdahl (Direction Cosines, Rotations, and
               Quaternions); this paper follows Kane’s approach closely.  Any reference on the
               subject will work.
