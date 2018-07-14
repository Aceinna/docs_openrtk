Test Area
============

.. contents:: Contents
    :local:

.. role::  raw-html(raw)
    :format: html






Formation of :math:`\vec{h}_{k}` from EKF states:
--------------------------------------------------

In the measurement model :math:`{\vec{h}_{k}}`, all terms are functions of the system states,
:math:`{\vec{x}_k}`.  The position and velocity elements of this vector come directly from the
position and velocity states, while :math:`{^{N}}{\Theta}{_{pred}^{B}}` is computed from
:math:`{^N}\vec{q}_{pred}^{B}`, as follows:

.. math::

    {^{⊥}{\phi}_{pred}^{B}} = atan2 \begin{bmatrix} {2 \cdot \begin{pmatrix} {q_{2} \cdot q_{3}+q_{0} \cdot q_{1}} \end{pmatrix},{q_{0}}^{2}-{q_{1}}^{2}-{q_{2}}^{2}+{q_{3}}^{2} } \end{bmatrix}

.. math::

    {^{⊥}{\theta}_{pred}^{B}} = -asin \begin{bmatrix} {2 \cdot \begin{pmatrix} {q_{1} \cdot q_{3}-q_{0} \cdot q_{2}} \end{pmatrix} } \end{bmatrix}

.. math::

    {^{N}{\psi}_{pred}^{⊥}} = atan2 \begin{bmatrix} {2 \cdot \begin{pmatrix} {q_{1} \cdot q_{2}+q_{0} \cdot q_{3}} \end{pmatrix},{q_{0}}^{2}+{q_{1}}^{2}-{q_{2}}^{2}-{q_{3}}^{2} } \end{bmatrix}


Observation Jacobian:

The Observation Jacobian, :math:`H`, is formulated from the measurement model, :math:`\vec{h}_{k}`.
The Observation Jacobian is a linearized version of the measurement model and is used to map the
measurements of :math:`{^{⊥}{\phi}_{pred}^{B}}`, :math:`{^{⊥}{\theta}_{pred}^{B}}`, and
:math:`{^{N}{\psi}_{pred}^{⊥}}` back to quaternion state, :math:`{^N}{\vec{q}}_{pred}^{B}`, ensuring
the EKF applies the state updates properly.  The Observation Jacobian is computed as follows:


.. math::

    H_{k} = \left.{ {\partial{\vec{h}}} \over {\partial{\vec{x}}} }\right|_{\vec{x}_{k},\vec{u}_{k}}


and results in a matrix of the form:

.. math::

    H_{k} = \begin{bmatrix} { { I_3 \\
                                0_3 \\
                                0_3
                              } \hspace{5mm}
                               { 0_3 \\
                                 I_3 \\
                                 0_3
                               } \hspace{5mm}
                               { 0_{3 \times 4} \\
                                 0_{3 \times 4} \\
                                 {\partial{h}\partial{q}}
                               } \hspace{5mm}
                               { 0_3 \\
                                 0_3 \\
                                 0_3
                               } \hspace{5mm}
                               { 0_3 \\
                                 0_3 \\
                                 0_3
                               }
             } \end{bmatrix}


where

.. math::

    {\partial{h}\partial{q}} = \begin{bmatrix} { H_{\phi} \\
                                                 H_{\theta} \\
                                                 H_{\psi}
                               } \end{bmatrix}


The three terms that make up :math:`{\partial{h}\partial{q}}` are found using the chain-rule for differentiation.  For roll, the equation becomes:

.. math::

    H_{\phi} = {{\partial{^{⊥}{\phi}_{pred}^{B}}} \over \partial{^{N}{\vec{q}}_{pred}^{B}}}
             = {{\partial{atan2 \begin{pmatrix} {y_{\phi}, x_{\phi}} \end{pmatrix}}} \over \partial{x_{\phi}}} \cdot {{\partial{x_{\phi}}} \over \partial{^{N}{\vec{q}}_{pred}^{B}}} +
               {{\partial{atan2 \begin{pmatrix} {y_{\phi}, x_{\phi}} \end{pmatrix}}} \over \partial{y_{\phi}}} \cdot {{\partial{y_{\phi}}} \over \partial{^{N}{\vec{q}}_{pred}^{B}}}


and results in:

.. math::

    H_{\phi} = \begin{pmatrix} {
                                 {2} \over {x_{\phi}^{2} + y_{\phi}^{2}}
               } \end{pmatrix} \cdot \begin{bmatrix} {
                                                       \begin{pmatrix} { x_{\phi} \cdot q_{1} - y_{\phi} \cdot q_{0} } \end{pmatrix} \hspace{5mm}
                                                       \begin{pmatrix} { x_{\phi} \cdot q_{0} + y_{\phi} \cdot q_{1} } \end{pmatrix} \hspace{5mm}
                                                       \begin{pmatrix} { x_{\phi} \cdot q_{3} + y_{\phi} \cdot q_{2} } \end{pmatrix} \hspace{5mm}
                                                       \begin{pmatrix} { x_{\phi} \cdot q_{2} - y_{\phi} \cdot q_{3} } \end{pmatrix} \hspace{5mm}
                                      } \end{bmatrix}

.. math::

    x_{\phi} = {q_{0}}^{2} - {q_{1}}^{2} - {q_{2}}^{2} + {q_{3}}^{2}

.. math::

    y_{\phi} = 2 \cdot \begin{pmatrix} { q_{2} \cdot q_{3}+q_{0} \cdot q_{1} } \end{pmatrix}


:math:`H_{\psi}` follows the same formulation as :math:`H_{\phi}`:

.. math::

    H_{\psi} = {{\partial{^{⊥}{\psi}_{pred}^{B}}} \over \partial{^{N}{\vec{q}}_{pred}^{B}}}
             = {{\partial{atan2 \begin{pmatrix} {y_{\psi}, x_{\psi}} \end{pmatrix}}} \over \partial{x_{\psi}}} \cdot {{\partial{x_{\psi}}} \over \partial{^{N}{\vec{q}}_{pred}^{B}}} +
               {{\partial{atan2 \begin{pmatrix} {y_{\psi}, x_{\psi}} \end{pmatrix}}} \over \partial{y_{\psi}}} \cdot {{\partial{y_{\psi}}} \over \partial{^{N}{\vec{q}}_{pred}^{B}}}


resulting in:

.. math::

    H_{\psi} = \begin{pmatrix} {
                                 {2} \over {x_{\psi}^{2} + y_{\psi}^{2}}
               } \end{pmatrix} \cdot \begin{bmatrix} {
                                                       \begin{pmatrix} { x_{\psi} \cdot q_{3} - y_{\psi} \cdot q_{0} } \end{pmatrix} \hspace{5mm}
                                                       \begin{pmatrix} { x_{\psi} \cdot q_{2} - y_{\psi} \cdot q_{1} } \end{pmatrix} \hspace{5mm}
                                                       \begin{pmatrix} { x_{\psi} \cdot q_{1} + y_{\psi} \cdot q_{2} } \end{pmatrix} \hspace{5mm}
                                                       \begin{pmatrix} { x_{\psi} \cdot q_{0} + y_{\psi} \cdot q_{3} } \end{pmatrix} \hspace{5mm}
                                      } \end{bmatrix}

.. math::

    x_{\psi} = {q_{0}}^{2} + {q_{1}}^{2} - {q_{2}}^{2} - {q_{3}}^{2}

.. math::

    y_{\psi} = 2 \cdot \begin{pmatrix} { q_{1} \cdot q_{2} + q_{0} \cdot q_{3} } \end{pmatrix}


Finally, for pitch the equation becomes:

.. math::

    H_{\theta} = {{\partial{^{⊥}{\theta}_{pred}^{B}}} \over \partial{^{N}{\vec{q}}_{pred}^{B}}}
               = -{{\partial{asin \begin{pmatrix} {u_{\theta}} \end{pmatrix}}} \over \partial{u_{\theta}}} \cdot {{\partial{u_{\theta}}} \over \partial{^{N}{\vec{q}}_{pred}^{B}}}


resulting in:

.. math::

    H_{\theta} = { { {2} \over \sqrt{ 1 - {u_{\theta}}^{2} } } \cdot { \begin{bmatrix} { {  q_{2} } \hspace{5mm}
                                                                                          { -q_{3} } \hspace{5mm}
                                                                                          {  q_{0} } \hspace{5mm}
                                                                                          { -q_{1} }
                                                                       } \end{bmatrix}
                                                                     }
                 }

.. math::

    u_{\theta} = 2 \cdot \begin{pmatrix} {
                                           q_{1} \cdot q_{3} - q_{0} \cdot q_{2}
                         } \end{pmatrix}





eeeeee

                         {^{N}{R}_{k-1}^{B}}
                         :math:``
                         \begin{pmatrix} {} \end{pmatrix}

                         \sin{ \begin{pmatrix} { {^{N}{\psi}^{⊥}} } \end{pmatrix} }
                         \cos{ \begin{pmatrix} { {^{N}{\psi}^{⊥}} } \end{pmatrix} }


Innovation (Measurement Error):

Once the measurements vectors are formed, the innovation (measurement error), :math:`\vec{\nu}_{k}`,
is computed:

.. math::

    \vec{\nu}_{k} = \vec{z}_{k} - \vec{h}_{k}


This result is used in the update stage of the EKF to generate the state error,
:math:`{\Delta\vec{x}}_{k}`, given the Kalman gain matrix.


Magnetometer vs GPS-Heading:

**These are just notes right now and may go elsewhere in the doc (probably in implementation section)**

How to combine :math:`{^N}{\psi}_{meas,gps}^{⊥}` and :math:`{^N}{\psi}_{meas,mag}^{⊥}`

    1) Don’t use :math:`{^N}{\psi}_{meas,mag}^{⊥}` if :math:`{^N}{\psi}_{meas,gps}^{⊥}`  is available

    2) Set :math:`{\nu}_{\psi} = 0` when GPS is valid and it is not time for a GPS update

    3) Create :math:`\Delta{^N}{\psi}_{meas,mag}^{⊥}` and use it for updates between GPS updates
	What if we are turning?  The latency may make the GPS heading less than ideal and affect :math:`\Delta{^N}{\psi}_{meas,mag}^{⊥}`.

    4) For vel < thresh, use mag, else use gps
	For vel < thresh, lock the heading update 

Measurement Covariance Values, R:

The measurement covariance is obtained in one of two ways:

    1) Value provided by the sensor (as for GPS messages)

    2) Calculated based on the underlying sensor noise


Setting this value properly is a key step toward a well-behaved EKF solution.  If the value of R is
too small the Kalman gain will be large, resulting in large EKF updates.  This may work well for a
static systems but will lead to errors in dynamic situations.  For example, when the Kalman gain is
large, a linear acceleration in the x-axis (even for a system that has not changed attitude) can be
misinterpreted as a change in the pitch.

Roll/Pitch Measurement Model and Covariance:

Static Case:

One way to determine the nominal (static) value for :math:`R` is to simulate the sensor noise as it is
passed through the measurement model.  For the roll and pitch angle, the models that convert the
accelerometer signal to angles are simply the *atan2* and *asin* functions.

Creating an accelerometer signal and passing it through the *asin* and *atan2* functions reveal the
noise on the measurements (during static periods), see Appendix R.  *Figure 5* and *Figure 6* show
that the standard-deviation of the roll measurement is highly dependent on the pitch angle
:math:`{^{⊥}{\theta}^{B}}` while the pitch standard-deviation is constant for all roll and
pitch angles ().


Figure 5: Roll and Pitch Standard-Deviation due to Accelerometer Noise

Figure 6: Roll and Pitch Standard-Deviation as a function of :math:`{^{⊥}{\theta}^{B}}`

In addition to finding the nominal values for :math:`{R}_{\phi}` and :math:`{R}_{\theta}` under
level conditions (:math:`{^{⊥}{\phi}^{B}} = {^{⊥}{\theta}^{B}}=0`), the change in :math:`{R}_{\phi}`
for different :math:`{^{⊥}{\theta}^{B}}` should be accounted for as well.  The solution was found
to become unstable (solution walked off at large pitch angles) if the change in :math:`{R}_{\phi}`
vs :math:`{^{⊥}{\theta}^{B}}` was not implemented.

One final note: the values in *Figure 5* and *Figure 6* are standard-deviation values.  To form the
:math:`R` matrix, the values must be squared as :math:`R` is based on the signal’s variance.
 
Heading Covariance:

The values for :math:`{R}_{\psi}` can also be based on magnetometer noise levels but, if set too
low, external magnetic disturbances can quickly pull the heading away from the correct value.  An
empirical approach can also be used: selecting a value so sudden magnetic disturbances (such as a
large truck pulling up besides the test vehicle) do not result in sudden changes in heading.
However, this can also have the negative effect that errors in the magnetic heading take some time
to recover.  The second approach was taken to determine an acceptable value for
:math:`{R}_{\psi,mag}` when operating as an AHRS.

When heading is available from the GPS, this is not an issue and :math:`{R}_{\psi,gps}` can be
selected in a different manner.  As described in the BestVel GPS message description, direction
accuracy is inversely proportional to vehicle speed.  The faster the system is traveling, the better
'the heading measurement.  This relationship can be used to set :math:`{R}_{\psi,gps}`.


At slow speeds (or a stop), :math:`{R}_{\psi,gps}` will get very large.  Two approaches to deal with
these cases are to

    1) Implement a yaw-lock.  Prevent a yaw update during these periods.
    2) Use the magnetometer solution at speeds below a certain threshold


Dynamic Case:

To find the appropriate :math:`R`-values, a Monte-Carlo approach was used.  For the …

Aided VG-Solution


Implementation

One of the challenges in implementing the Extended Kalman Filter comes from determining the quality
of the measurement and setting the measurement covariance, :math:`R`, appropriately.  As mentioned
previously, roll and pitch measurements are nominally computed from static accelerometer noise
levels.  However, when the system is moving, the accelerometer signal may also contains linear and
centripetal acceleration components (as well as system vibrations).  These components distort the
gravity measurement and affect the roll and pitch estimates as the system does not know if the
measured angles are changing due to a change in attitude (gravity) or a linear acceleration.


In practice, discerning between the gravity and motion (and adjusting :math:`R` accordingly) has the
potential to improve the attitude results.  In this case, adjusting the value of :math:`R` during
acceleration periods (increasing the value) reduces the effect of the acceleration on the state
update.  When the system returns to a static (non-accelerating) state, the value of :math:`R` can be
reduced to the nominal value, which results in a higher Kalman gain  and more aggressive updates.


A simple approach to implementing this is to compare the magnitude of the accelerometer signal
against the expected magnitude of gravity.  When an appreciable difference is detected (more than
typical sensor/system noise would cause), the value of :math:`R` is increased.  When the difference
is removed, the value of :math:`R` is restored.  While simple in theory, this is more difficult in
practice.  Why?  To avoid single point errors (mitigated by using the signal only after a certain
amount of time elapses).  To ensure the gain drops before the measurement is used (filter properly).


Other things to improve performance:

    1) Limit the innovation error, :math:`\vec{\nu}_{k}`.  This reduces the error going into the EKF
    Update resulting in smaller state updates.  Setting the error limit this way is justified as the
    errors are typically only large during periods of acceleration, which are erroneous anyway.

    2) Change R based on the quality of the measurement.  Some measurements (particularly GPS
    measurements) are provided along with a measure of their variance.  When available, these values
    can be used to adjust :math:`R`.  Other measurements do not provide this information and the
    user is left to set :math:`R` based on intuition or simulation.  For instance, as mentioned
    above, :math:`\phi` and :math:`\theta` are affected by acceleration; :math:`{R}_{\phi}` and
    :math:`{R}_{\theta}` should be increased during these periods.  :math:`{R}_{\psi}` is affected
    by turns about the z-axis and :math:`{R}_{\psi}` should be increased accordingly to account for
    lag and other effects.

    3) Combining heading from two sources. 	Need to think of how to combine these two measurements

    4) Don’t use mag heading when GPS valid?

    5) Latency in GPS message: Any latency in obtaining, parsing, and providing GPS messages should
    be accounted for by either 1) adjusting R or 2) accounting for the latency.  For instance, if
    the GPS messages is consistently late by DT seconds, then the heading can be adjusted by a
    formula such as:

.. math::

    \psi_{GPS} = \psi_{GPS} - \dot{\psi} \cdot \Delta{T}

    6) Much of the math on which the EKF is based consists of sparse matrices.  Using algorithms
    that take advantage of sparse matrices make the algorithms run much faster and permit higher
    execution rates.  For the most part, only the *P*-matrix needs to have all its elements
    considered.

    7) The INS algorithm makes use of a sequential approach to solving for the states.  From an
    execution point-of-view this makes the runtime of the algorithm significantly less as only 3x3
    matrix inverses are required to solve for the state updates

 
Test Results

 
Appendix:
Cross-Product Matrix:
The cross-product between two 3x1 vectors is calculated as:
\vec{a} \timesb ⃑=|■(i ̂&j ̂&k ̂@a_x&a_y&a_z@b_x&b_y&b_{z} )|=■(i ̂ \cdot (a_y \cdot b_{z}-a_z \cdot b_y )@-j ̂ \cdot (a_x \cdot b_{z}-a_z \cdot b_x )@+k ̂ \cdot (a_x \cdot b_y-a_y \cdot b_x ) )
=[■(0&-a_z&a_y@a_z&0&-a_x@-a_y&a_x&0)] \cdot {■(b_x@b_y@b_{z} )}
The resulting cross-product matrix is:
[\vec{a} \times]=[■(0&-a_z&a_y@a_z&0&-a_x@-a_y&a_x&0)]
Resulting in the final expression:
\vec{a} \timesb ⃑=[\vec{a} \times] \cdot \vec{b}
This terminology can be used to simplify expressions for larger matrices.  For example, Ω can be rewritten as
Ω=[■(0&-ω ⃑^T@ω ⃑&[ω ⃑ \times]^T )]=[■(0&-ω ⃑^T@ω ⃑&-[ω ⃑ \times] )]
where [ω ⃑ \times] is the cross-product matrix based on the angular velocity vector, ω ⃑^B:
[ω ⃑ \times]≝[■(0&-ω_z&ω_y@ω_z&0&-ω_x@-ω_y&ω_x&0)]



 
Process Jacobians:
Only the less obvious derivatives are included here.
Derivation of ∂v∂q:
∂v∂q≝2 \cdot ∆t \cdot (■([■(■(■(q_{0}@q_{3}@-q_{2} )&■(q_{1}@q_{2}@q_{3} ))&■(■(-q_{2}@q_{1}@-q_{0} )&■(-q_{3}@q_{0}@q_{1} )))] \cdot a ̂_(motion x)^B+⋯@[■(■(■(-q_{3}@q_{0}@q_{1} )&■(q_{2}@-q_{1}@q_{0} ))&■(■(q_{1}@q_{2}@q_{3} )&■(-q_{0}@-q_{3}@q_{2} )))] \cdot a ̂_(motion y)^B+⋯@[-■(■(■(q_{2}@q_{1}@q_{0} )&■(q_{3}@-q_{0}@-q_{1} ))&■(■(q_{0}@q_{3}@-q_{2} )&■(q_{1}@q_{2}@q_{3} )))] \cdot a ̂_(motion z)^B ))
Form the matrix Q ̅
Q ̅=[■(■(■(q_{1}@q_{2}@q_{3} )&■(q_{0}@q_{3}@-q_{2} ))&■(■(-q_{3}@q_{0}@q_{1} )&-■(q_{2}@q_{1}@q_{0} )))]=[■(\vec{q}_{v}&q_{0}⋅I_3+[\vec{q}_{v} \times] )]
∂v∂q≝2 \cdot ∆t \cdot (■(Q ̅ \cdot [■(■(0&1@1&0)&■(0&0@0&0)@■(0&0@0&0)&■(0&1@-1&0))] \cdot a ̂_(motion x)^B+⋯@Q ̅ \cdot [■(■(0&0@0&0)&■(1&0@0&-1)@■(1&0@0&1)&■(0&0@0&0))] \cdot a ̂_(motion y)^B+⋯@Q ̅ \cdot [■(■(0&0@0&0)&■(0&1@1&0)@■(0&-1@1&0)&■(0&0@0&0))] \cdot a ̂_(motion z)^B ))
∂v∂q≝2 \cdot ∆t \cdot Q ̅ \cdot (■([■(■(0&1@1&0)&■(0&0@0&0)@■(0&0@0&0)&■(0&1@-1&0))] \cdot a ̂_(motion x)^B+⋯@[■(■(0&0@0&0)&■(1&0@0&-1)@■(1&0@0&1)&■(0&0@0&0))] \cdot a ̂_(motion y)^B+⋯@[■(■(0&0@0&0)&■(0&1@1&0)@■(0&-1@1&0)&■(0&0@0&0))] \cdot a ̂_(motion z)^B ))
The terms inside the parenthesis can be written as:
[■(■(0&1@1&0)&■(0&0@0&0)@■(0&0@0&0)&■(0&1@-1&0))] \cdot a ̂_(motion x)^B+[■(■(0&0@0&0)&■(1&0@0&-1)@■(1&0@0&1)&■(0&0@0&0))] \cdot a ̂_(motion y)^B+[■(■(0&0@0&0)&■(0&1@1&0)@■(0&-1@1&0)&■(0&0@0&0))] \cdot a ̂_(motion z)^B
Expanding the equation and writing the resultant matrix using vector and cross-product terms results in the final form for ∂v∂q:
∂v∂q≝2 \cdot ∆t \cdot Q ̅⋅[■(0&(a ̂_motion^B )^T@a ̂_motion^B&-[a ̂_motion^B \times] )]


Compute ∂q∂ω_bias
Expand
-∆t/2 \cdot Ω_(noise,k-1) \cdot q ⃑_(k-1)
And differentiate wrt the bias terms leads to:
Q^*≝2 \cdot ∆t \cdot [■(■(q_{1}@-q_{0} )&■(q_{2}@q_{3} )&■(q_{3}@-q_{2} )@■(-q_{3}@q_{2} )&■(-q_{0}@-q_{1} )&■(q_{1}@-q_{0} ))]=-Ξ_(k-1)

The second term, Q^*, is:
Q^*≝[■(■(q_{1}@-q_{0} )&■(q_{2}@q_{3} )&■(q_{3}@-q_{2} )@■(-q_{3}@q_{2} )&■(-q_{0}@-q_{1} )&■(q_{1}@-q_{0} ))]=[■((\vec{q}_{v} )^T@-(q_{0}⋅I_3+[\vec{q}_{v} \times]) )]=-Ξ_(k-1)


Software Implementation

Initialization:

a_sum=∑_(k=1)^N▒a ⃑_k^B
m_sum=∑_(k=1)^N▒m ⃑_k^B

After N data-points are collected, average data and from the ICs:
a ̅^B=a_sum/N
m ̅^B=m_sum/N

Compute the gravity and magnetic-field unit-vectors:
g ̂^B=-a ̅^B/|a ̅^B |
m ̂^B=-m ̅^B/|m ̅^B |

Find the components of the magnetic-field that are parallel and perpendicular to the gravity vector:
m ⃑_(∥g)^B=(m ̂^B⋅g ̂^B ) \cdot g ̂^B
m ⃑_(⊥g)^B=m ̂^B-m ⃑_(∥g)^B

Form the axes of the NED-frame from the magnetic and gravity field vectors.  The D-axis is parallel to the gravity vector while the N-axis is parallel to the magnetic field vector that is perpendicular to the gravity vector:
z ̂_N^B=g ̂^B
x ̂_N^B=(m ⃑_(⊥g)^B)/|m ⃑_(⊥g)^B |
〖y ̂_N^B=z ̂_N^B \timesx ̂〗_N^B

The transformation matrix, (_^N)R_^B , is formed from these unit-vectors:
(_^N)R_^B =[■((x ̂_N^B )^T@(y ̂_N^B )^T@(z ̂_N^B )^T )]=[■(x ̂_B^N&y ̂_B^N&z ̂_B^N )]

The attitude quaternion, (_^N)q_^B , can be calculated from (_^N)R_^B :
(_^N)q_^B =f((_^N)R_^B )

The initial state-vector is formed from these values:
\vec{x}_0={■(■(r@v@(_^N)q_^B  )@ω ⃑_bias@a ⃑_bias )}



Appendix Q:

Quaternion process covariance:
〖w_q \cdot {\vec{w}_{q}}^T=(Δt/2)〗^{2} \cdot (Ξ \cdot Σ_ω \cdot Ξ^T )

The rate-sensor noise is treated as a stationary process, so the time subscript, k, can be dropped from the noise terms.  However, the attitude does change with time and k should remain on the quaternion terms (removed here for ease of reading).  Additionally, the sensor noise is assumed to be the same for all sensor channels.
Ξ≡[■(-〖\vec{q}_{v}〗^T@q_{0} \cdot I_3+[\vec{q}_{v} \times] )]
〖w_q \cdot {\vec{w}_{q}}^T=(Δt/2)〗^{2} \cdot [■(■(-q_{1}&-q_{2}@q_{0}&-q_{3} )&■(-q_{3}@q_{2} )@■(q_{3}&q_{0}@-q_{2}&q_{1} )&■(-q_{1}@q_{0} ))] \cdot [■(〖σ_ω〗^{2}&0&0@0&〖σ_ω〗^{2}&0@0&0&〖σ_ω〗^{2} )] \cdot [■(■(-q_{1}&q_{0} )&■(q_{3}&-q_{2} )@■(-q_{2}&-q_{3} )&■(q_{0}&q_{1} )@■(-q_{3}&q_{2} )&■(-q_{1}&q_{0} ))]
〖w_q \cdot {\vec{w}_{q}}^T=(Δt/2)〗^{2} \cdot 〖σ_ω〗^{2} \cdot [■(■(-q_{1}&-q_{2}@q_{0}&-q_{3} )&■(-q_{3}@q_{2} )@■(q_{3}&q_{0}@-q_{2}&q_{1} )&■(-q_{1}@q_{0} ))] \cdot [■(■(-q_{1}&q_{0} )&■(q_{3}&-q_{2} )@■(-q_{2}&-q_{3} )&■(q_{0}&q_{1} )@■(-q_{3}&q_{2} )&■(-q_{1}&q_{0} ))]
Performing the multiplication (and crossing out terms that cancel) results in:
\Sigma_{q} = ((σ_ω \cdot ∆t)/2)^{2} \cdot [■(■(1-{q_{0}}^{2}&-q_{0} \cdot q_{1}@-q_{0} \cdot q_{1}&1-{q_{1}}^{2} )&■(-q_{0} \cdot q_{2}&-q_{0} \cdot q_{3}@-q_{1} \cdot q_{2}&-q_{1} \cdot q_{3} )@■(-q_{0} \cdot q_{2}&-q_{1} \cdot q_{2}@-q_{0} \cdot q_{3}&-q_{1} \cdot q_{3} )&■(1-{q_{2}}^{2}&-q_{2} \cdot q_{3}@-q_{2} \cdot q_{3}&1-{q_{3}}^{2} ))]

Rate-bias Process-Covariance:
	w ⃑_(q,k-1)	=-∆t/2 \cdot {■(■(-ω_(noise x,k-1)^B \cdot q_(1,k-1)-ω_(noise y,k-1)^B \cdot q_(2,k-1)-ω_(noise z,k-1)^B \cdot q_(3,k-1)@ω_(noise x,k-1)^B \cdot q_(0,k-1)+ω_(noise z,k-1)^B \cdot q_(2,k-1)-ω_(noise y,k-1)^B \cdot q_(3,k-1) )@■(ω_(noise y,k-1)^B \cdot q_(0,k-1)-ω_(noise z,k-1)^B \cdot q_(1,k-1)+ω_(noise x,k-1)^B \cdot q_(3,k-1)@ω_(noise z,k-1)^B \cdot q_(0,k-1)+ω_(noise y,k-1)^B \cdot q_(1,k-1)-ω_(noise x,k-1)^B \cdot q_(2,k-1) ))}
		=-∆t/2 \cdot [■(■(-q_(1,k-1)&-q_(2,k-1)@q_(0,k-1)&-q_(3,k-1) )&■(-q_(3,k-1)@q_(2,k-1) )@■(q_(3,k-1)&q_(0,k-1)@-q_(2,k-1)&q_(1,k-1) )&■(-q_(1,k-1)@q_(0,k-1) ))] \cdot {■(ω_(noise x,k-1)^B@ω_(noise y,k-1)^B@ω_(noise z,k-1)^B )}
		=-∆t/2 \cdot [■(-〖\vec{q}_{v}〗^T@q_{0} \cdot I_3+[\vec{q}_{v} \times] )] \cdot ω ⃑_(noise,k-1)^B

		=-∆t/2 \cdot Ξ \cdot ω ⃑_(noise,k-1)^B 
Velocity Process-Covariance:
Q_v=\vec{w}_{v,k-1}^{N} \cdot {\vec{w}_{v,k-1}^{N}}^T
\vec{w}_{v,k-1}^{N}=-{{^{N}{R_{k-1}}^{B}}} \cdot a ⃑_(noise,k-1)^B \cdot ∆t
Q_v=(-{{^{N}{R_{k-1}}^{B}}} \cdot a ⃑_(noise,k-1)^B \cdot ∆t) \cdot (-{{^{N}{R_{k-1}}^{B}}} \cdot a ⃑_(noise,k-1)^B \cdot ∆t)^T
Q_v=(-∆t)^{2} \cdot {{^{N}{R_{k-1}}^{B}}} \cdot a ⃑_(noise,k-1)^B \cdot 〖a ⃑_(noise,k-1)^B〗^T \cdot 〖{{^{N}{R_{k-1}}^{B}}} 〗^T
Q_v=(-∆t)^{2} \cdot {{^{N}{R_{k-1}}^{B}}} \cdot [■({\sigma_{a}}^{2}&0&0@0&{\sigma_{a}}^{2}&0@0&0&{\sigma_{a}}^{2} )] \cdot 〖{{^{N}{R_{k-1}}^{B}}} 〗^T
Q_v=(-∆t \cdot \sigma_{a} )^{2} \cdot {{^{N}{R_{k-1}}^{B}}} \cdot [■(1&0&0@0&1&0@0&0&1)] \cdot 〖{{^{N}{R_{k-1}}^{B}}} 〗^T
Q_v=(-∆t \cdot \sigma_{a} )^{2} \cdot {{^{N}{R_{k-1}}^{B}}} \cdot 〖{{^{N}{R_{k-1}}^{B}}} 〗^T
Since {{^{N}{R_{k-1}}^{B}}}  is orthonormal
{{^{N}{R_{k-1}}^{B}}} \cdot 〖{{^{N}{R_{k-1}}^{B}}} 〗^T={{^{N}{R_{k-1}}^{B}}} \cdot 〖{{^{N}{R_{k-1}}^{B}}} 〗^(-1)=I_3
Q_v=(-∆t \cdot \sigma_{a} )^{2} \cdot I_3
 
Appendix Trigonometric function Derivatives:
For θ=atan2(y,x), the derivative ∂θ/∂q, where x and y are functions of q, is:
	∂θ/∂q	=(∂atan2(y,x))/∂x \cdot ∂x/∂q+(∂atan2(y,x))/∂y \cdot ∂y/∂q
		=(-y)/(x^{2}+y^{2} ) \cdot ∂x/∂q+(-y)/(x^{2}+y^{2} ) \cdot ∂y/∂q

For θ=-asin(u), the derivative ∂θ/∂q, where x and y are functions of q, is:
	∂θ/∂q	=-(∂ asin⁡(u))/∂u \cdot ∂u/∂q
		=(-1)/\sqrt{1 - u^{2}} \cdot ∂u/∂q

 
Least-Square Hard/Soft-Iron Parameter Estimation:
The hard and soft-iron parameters corresponding to a given system are estimated (for a two-dimensional problem) using the Magnetic-Alignment process described earlier.  After the maneuver is performed, the x and y-magnetic field measurement data is processed to determine parameters that best describe the resulting ellipse.
Two methods can be used to find these parameters.  An elegant and interesting approach to the least-squares solution was developed by Andrew W. Fitzgibbon, Maurizio Pilu, and Robert B. Fisher.  Entitled Direct least-squares fitting of ellipses, and published in IEEE Transactions on Pattern Analysis and Machine Intelligence, 21(5), 476--480, May 1999.  Matlab code and an extension to improve numerical accuracy are found at http://homepages.inf.ed.ac.uk/rbf/CVonline/LOCAL_COPIES/FITZGIBBON/ELLIPSE/.
However this method requires solving for eigenvalues, which is numerically intensive.  Instead a least-squares approach was selected based on general quadratic form of the ellipse equation.
A \cdot x^{2}+B \cdot x \cdot y+C \cdot y^{2}+D \cdot x+E \cdot y+F=0
The least-squares solution was found by first forming an equation representing the error for a given data-point
ε_i=A \cdot 〖x_i〗^{2}+B \cdot x_i \cdot y_i+C \cdot 〖y_i〗^{2}+D \cdot x_i+E \cdot y_i+F
then computing the summation of the errors squared
ε_T=∑_(i=1)^n▒〖ε_i〗^{2}
and, finally, minimizing the summation with respect to each coefficient
〖dε〗_T/dA=0
etc.
This resulting system of equations can be written in matrix form as A_LS \cdot x=b_LS, where the constituent matrices are:
A_LS=[■(■(∑▒〖〖x_i〗^{2} \cdot 〖y_i〗^{2} 〗@∑▒〖x_i \cdot 〖y_i〗^3 〗@■(∑▒〖〖x_i〗^{2} \cdot y_i 〗@∑▒〖x_i \cdot 〖y_i〗^{2} 〗@∑▒〖x_i \cdot y_i 〗))&■(∑▒〖x_i \cdot 〖y_i〗^3 〗@∑▒〖y_i〗^4 @■(∑▒〖x_i \cdot 〖y_i〗^{2} 〗@∑▒〖y_i〗^3 @∑▒〖y_i〗^{2} ))&■(■(∑▒〖〖x_i〗^{2} \cdot y_i 〗@∑▒〖x_i \cdot 〖y_i〗^{2} 〗@■(∑▒〖x_i〗^{2} @∑▒〖x_i \cdot y_i 〗@∑▒x_i ))&■(∑▒〖x_i \cdot 〖y_i〗^{2} 〗@∑▒〖y_i〗^3 @■(∑▒〖x_i \cdot y_i 〗@∑▒〖y_i〗^{2} @∑▒y_i ))&■(∑▒〖x_i \cdot y_i 〗@∑▒〖y_i〗^{2} @■(∑▒x_i @∑▒y_i @n))))]
b_LS=[■(∑▒〖〖x_i〗^3 \cdot y_i 〗@∑▒〖〖x_i〗^{2} \cdot 〖y_i〗^{2} 〗@■(∑▒〖x_i〗^3 @∑▒〖〖x_i〗^{2} \cdot y_i 〗@∑▒〖x_i〗^{2} ))]
and the coefficient matrix
x=[■(■(A@B)@■(C@D)@■(E@F))]
The coefficients can be found via Gaussian elimination.
Based on test data, both solutions provide consistent results.  This is possible as data from a complete 360 degree rotation is used for the data set.  If the system had transited only a small arc then the method described by Fitzgibbon et al. is preferred.

 


 
Appendix
Example sensor values for a single unit captured over a half-hour in a noisy environment (at my desk)
Sensor
	Min	Max	Mean	Std Dev	Allan Var
GPS Position	X
	Y
	Z
GPS Velocity	X
	Y
	Z
Angular Rate Sensor [deg/sec]	X	-0.288	0.365	2. 4948e-2	8.42685e-2
	Y	-0.442	0.481	-4.8527e-3	9.04376e-2
	Z	-0.558	0.250	-0.13971	9.80244e-2
Accelerometer [g]	X	5.8e-3	9.5e-3	7.7358e-3	4.41395e-4
	Y	-4.0e-3	1.5e-3	-1.457e-3	5.80786e-4
	Z	-1.0052	-0.9964	-1.000723	6.52203e-4
Magnetometer	X	0.21271	0.21912	0.21632	7.4808e-4
	Y	-0.1651	-0.15442	-0.16002	1.1478e-3
	Z	0.28656	0.29297	0.28945	7.6077e-4



Others:
