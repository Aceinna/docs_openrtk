Test Area
============

.. contents:: Contents
    :local:

.. role::  raw-html(raw)
    :format: html

Velocity State-Transition Model
--------------------------------

The velocity propagation equation is based on the following first-order model:

.. math::

    \vec{v}_{k} = \vec{v}_{k-1} + \dot{\vec{v}}_{k-1} \cdot dt

:math:`\dot{\vec{v}}_{k-1}` is an estimate of system motion (linear-acceleration corrected for gravity) and is formed from
the accelerometer signal with estimated accelerometer-bias and gravity removed.

.. math::

    \vec{a}_{motion,k-1} = \vec{a}_{meas,k-1} - \hat{a}_{bias,k-1} - \vec{a}_{grav}


Substituting this expression (along with the noise term) into the velocity propagation equation, and explicitly stating the
frames in which the signals are available, leads to:

.. math::

    \vec{v}_{k}^N = \vec{v}_{k-1}^N + (\vec{a}_{motion,k-1}^N-{^{N}{R_{k-1}}^{B}} \cdot \vec{a}_{noise}^{B} ) \cdot d

where

.. math::

    \vec{a}_{motion,k-1}^N = {^{N}{R_{k-1}}^{B}} \cdot (\vec{a}_{meas,k-1}^B-\hat{a}_{bias,k-1}^B ) - \vec{a}_{grav}^{N}


The velocity process-noise vector resulting from accelerometer noise is:

.. math::

    \vec{w}_{v,k-1}^{N} = -{^{N}{R_{k-1}}^{B}} \cdot \vec{a}_{noise}^{B} \cdot dt


leading to the final formulation for the velocity state-transition model:

.. math::

    \vec{v}_{k}^N = \vec{v}_{k-1}^N + \vec{a}_{motion,k-1}^N \cdot dt + \vec{w}_{v,k-1}^{N}


The velocity process noise vector is used to compute the elements of the process covariance matrix (Q) related to the velocity
estimate, as follows:

.. math::

    \Sigma_{v} = \vec{w}_{v,k-1}^{N} \cdot {\vec{w}_{v,k-1}^{N}}^{T}

By making the assumption that all axes have the same noise characteristics ({\sigma_{a}}^{2}) and manipulating the expression,
the result can be simplified to the following (Appendix Q):

.. math::

    \Sigma_{v} = (\sigma_{a} \cdot dt)^{2} \cdot I_3


Position State-Transition Model
--------------------------------

The position process model is based on the following first-order model:

.. math::

    \vec{r}_{k} = \vec{r}_{k-1} + \dot{\vec{r}}_{k-1} \cdot dt

where \dot{\vec{r}}_{k-1} is equal to the estimated velocity state, \vec{v}_{k-1}.  Substituting in the velocity term (including noise) results in:

.. math::

    \vec{r}_{k} = \vec{r}_{k-1} + \vec{v}_{k-1} \cdot dt + \vec{w}_{r,k-1}

\vec{w}_{r,k-1} is the process noise associated with the position state-transition model (directly related to the velocity process noise):

	\vec{w}_{r,k-1}	= \vec{w}_{v,k-1}^{N} \cdot dt

		= {{^{N}{R_{k-1}}^{B}}} \cdot \vec{a}_{noise}^{B} \cdot {dt}^{2}

Like the previous process models, this expression is used to compute the elements of the process covariance matrix (Q) related to the position estimate:

\Sigma_{r} = \vec{w}_{r,k-1} \cdot {\vec{w}_{r,k-1}}^{T}


By making the assumption that all axes have the same noise characteristics ({\sigma_{a}}^{2}), \Sigma_{r} simplifies to:

\Sigma_{r} = ({\sigma_{a} \cdot dt}^{2} )^{2} \cdot I_3

See Appendix Q for further information.


Rate and Acceleration Bias State-Transition Models
---------------------------------------------------

The process models for the bias terms are based on the assumption that bias is made up of two components:

    1) A constant bias offset (\vec{\omega}_{offset}^{B})

    2) A randomly varying component superimposed on the offset (\vec{\omega}_{drift}^{B}) based on the measured bias-instability value of the sensor

For the rate-sensor, the bias model is

\vec{\omega}_{bias}^{B} = \vec{\omega}_{offset}^{B} + \vec{\omega}_{drift}^{B}

The drift model follows a random-walk process FN, i.e. the drift value wanders according to a Gaussian distribution.

\vec{\omega}_{drift,k}^{B} = \vec{\omega}_{drift,k-1}^{B} + \dot{\vec{\omega}}_{drift,k-1}^{B} \cdot dt

where

\dot{\vec{\omega}}_{drift,k-1}^{B} = N(0,\sigma_(dd,ω)^{2} )

Note: the subscript dd stands for drift-dot.  Based on this model, the process variance for \vec{\omega}_{drift}^{B} at time, t, is given by:

\sigma_{d,\omega}^{2}(t)=[(\sigma_(dd,ω) \cdot \sqrt{dt}) \cdot \sqrt{t}]^{2}

Based on an empirical study, \sigma_(dd,ω) is related to the BI and ARW as follows:

\sigma_(dd,ω)=(2 \cdot π)/ln(2) \cdot {BI}^{2}/ARW

To find the rate-bias process noise covariance, set t = dt in the model (above), resulting in:
{\Sigma_{ωb}_{d,\omega}^{2} (dt) \cdot I_3=(\sigma_(dd,ω) \cdot dt)^{2} \cdot I_3

The accelerometer drift model mirrors this formulation.

{\Sigma_{ab}_(d,a}^{2} (dt) \cdot I_3=(\sigma_(dd,a) \cdot dt)^{2} \cdot I_3


Process Jacobian
-----------------

As the system is nonlinear, the vector \vec{f} cannot be used to propagate the covariance matrix, P.  Instead the Process Jacobian, F, (a linearized version of the state-transition vector) is computed at each time step (based on the current system states) to propagate P forward in time:

F_{k-1} = ├ {{∂\vec{f}} \over {∂\vec{x}}┤| _{\vec{x}_(k-1),\vec{u}_{k-1}}

This requires taking the derivative of each state-equation with respect to each state.  Each row of the Jacobian corresponds to a specific state-equation; each column of the matrix corresponds to a specific system state.  Performing this operation results in:

MATRIX GOES HERE

The one new term in the matrix,  ∂v∂q, is derived in the Appendix B and listed below.

∂v∂q ≡ 2 \cdot Q ̅_F⋅[■(0&(\vec{a}^{B} )^T@\vec{a}^{B}&-[\vec{a}^{B} \times] )]
where Q ̅_F is:
Q ̅_F=[■(■(■(q_{1}@q_{2}@q_{3} )&■(q_{0}@q_{3}@-q_{2} ))&■(■(-q_{3}@q_{0}@q_{1} )&-■(q_{2}@q_{1}@q_{0} )))]=[■(\vec{q}_{v}&q_{0}⋅I_3+[\vec{q}_{v} \times] )]
and
\vec{a}^{B}=\vec{a}_{meas}^{B}-\vec{a}_{bias}^{B}


Process Noise Covariance Matrix
--------------------------------

The process covariance acts as a weighting matrix for the system process.  It relates the covariance between the ith and jth element of each process-noise vector.  It is defined as:

Σ_ij=cov(\vec{x}_i,\vec{x}_j )=E[(\vec{x}_i-\mu_i ) \cdot (\vec{x}_j-\mu_j )]

A Kalman Filter essentially multiplies Gaussians to form state estimates.  Q is a measure of the width of the Gaussian distribution for each state.  The wider the distribution, the more uncertainty that exists in the model.  This leads to the update affecting the state more than if it had a tighter distribution, which cause the updates to have less influence on each state.

Based on the state process-noise vectors, \vec{w}_{k} (found above), the Process Noise Covariance Matrix is:

MATRIX GOES HERE

The individual process covariance (derived in the sections above) are repeated here:

\Sigma_{r} = (\sigma_{a} \cdot {dt}^{2} )^{2} \cdot I_3

\Sigma_{v} =(\sigma_{a} \cdot dt)^{2} \cdot I_3

\Sigma_{q} = ((σ_ω \cdot dt)/2)^{2} \cdot [■(■(1-{q_{0}}^{2}&-q_{0} \cdot q_{1}@-q_{0} \cdot q_{1}&1-{q_{1}}^{2} )&■(-q_{0} \cdot q_{2}&-q_{0} \cdot q_{3}@-q_{1} \cdot q_{2}&-q_{1} \cdot q_{3} )@■(-q_{0} \cdot q_{2}&-q_{1} \cdot q_{2}@-q_{0} \cdot q_{3}&-q_{1} \cdot q_{3} )&■(1-{q_{2}}^{2}&-q_{2} \cdot q_{3}@-q_{2} \cdot q_{3}&1-{q_{3}}^{2} ))]

\Sigma_{\omega b} = (\sigma_(dd,ω) \cdot dt)^{2} \cdot I_3

\Sigma_{ab} = (\sigma_(dd,a) \cdot dt)^{2} \cdot I_3


Measurement Models
-------------------

It is possible to choose among various measurement models for a given EKF implementation.  The particular model is selected based on many factors, one being the limitations of the available measurements.  This formulation was selected due to the incomplete knowledge of the magnetic environment of the system  and uses the available sensor information as follows:
	Accelerometers “level” the system (used to compute {^{⊥}}{\phi}{_{meas}^{B}}  and {^{⊥}}{\theta}{_{meas}^{B}}  )
	Magnetometers and/or GPS heading information align the ⊥-frame with true or magnetic north {^{N}}{\psi}{^{⊥}}
	GPS position and velocity measurements update the position and velocity estimates (\vec{r}^{N} and \vec{v}^{N})
Based upon these steps, the measurement vector, \vec{z}_{k}, is formed:
\vec{z}_{k} = {■(\vec{r}_{GPS}^{N}@\vec{v}_{GPS}^{N}@{^{N}}{\Theta}{_{meas}^{B}}  )}
with the corresponding measurement model, \vec{h}_{k}:
\vec{h}_{k} = {■(\vec{r}_{pred}^{N}@\vec{v}_{pred}^{N}@{^{N}}{\Theta}{_{pred}^{B}} )}
Both {^{N}}{\Theta}{_{meas}^{B}}  and {^{N}}{\Theta}{_{pred}^{B}} are 3x1 column vectors containing the roll, pitch, and heading values .
Formation of \vec{z}_{k} from sensor measurements:
The measurement vector, \vec{z}_{k} is comprised of position, velocity, and attitude information as defined above.  However, only the GPS velocity is available directly from measurements; other information must be derived from sensor readings using the relationship described below.
Roll and Pitch Measurements:
Roll and pitch values are computed from the accelerometer signal.  Under static conditions, measurements made by the accelerometer consists solely of gravity and sensor noise.  Along the axis pointed in the direction of gravity, the sensor measures -1 [g].  This is due to the proof-mass being pulled in the direction of gravity, which is equivalent to a deceleration of 1 [g] in the absence of gravity.

\vec{a}_{meas} = \vec{a}_{grav} = -\vec{g}

Static roll and pitch values are determined by noting that gravity is constant in the N-Frame (⊥-Frame):

\vec{g}^{N} = \vec{g}^{⊥} = {■(0@0@1)}

and can be transformed into the body frame through {^{B}{R}^{⊥}}:

\vec{g}^{B} = {^{B}{R}^{⊥}} \cdot \vec{g}^{⊥}=({^{⊥}{R}^{B}}  )^T \cdot \vec{g}^{⊥}=({^{⊥}{R}^{B}}  )^T \cdot {■(0@0@1)}

By using the definition of {^{⊥}{R}^{B}}  (pg. 2) and expanding the equation, the accelerometer measurements can be related to roll and pitch angles:

-\vec{a}_{meas}^{B}={■(-a_{mx}^{B}@-a_{my}^{B}@-a_mz^B )}=\vec{g}^{B}={■(-sin({^{⊥}{\theta}^{B}}  )@cos({^{⊥}{\theta}^{B}}  ) \cdot sin({^{⊥}{\phi}^{B}}  )@cos({^{⊥}{\theta}^{B}}  ) \cdot cos({^{⊥}{\phi}^{B}}  ) )}
From this result, the angles corresponding to the accelerometer signal are found:

{^{⊥}}{\phi}{_{meas}^{B}} =atan2(-a_{my}^{B},-a_mz^B )

{^{⊥}}{\theta}{_{meas}^{B}}  =-asin(-\hat{a}_{mx}^{B} )

where, \hat{a}_{mx}^{B} is the x-axis acceleration value normalized by the total acceleration magnitude:
\hat{a}_{mx}^{B}=(a_mx^B)/‖\vec{a}_{meas}^{B} ‖
Normalization of the y and z-axis accelerometer values can be performed but is not required as the atan function uses the ratio of the two (the normalization factor cancels out).
Heading Measurements:
Heading measurements are determined from the following:
	Magnetometers
	GPS Velocity
Magnetometer-Based Heading:
Magnetometers measure the local magnetic field at a high DRs but the readings can be affected by hard and soft-iron disturbances in the system or by changes in the external magnetic field.  Hard and soft-iron effects are local to the system and can be accounted for; external field disturbances cannot be corrected.
Adjustment of the magnetic field measurement for hard/soft-iron disturbances can be performed according to the following equation:
\vec{m}_{corr}^{B} = R_{SI} \cdot S_{SI} \cdot {R_{SI}}^{T} \cdot (\vec{m}_{meas}^{B} - \vec{m}_{bias}^{B} - \vec{m}_{HI}^{B} )

where \vec{m}_{meas}^{B} is the measured magnetic field vector in the body-frame, \vec{m}_{HI}^{B} is the hard-iron disturbance, and R_SI and S_{SI} are the soft-iron disturbances.  Note: for this analysis the magnetometer bias is neglected; assumed to be negligible or lumped in with the hard-iron.

Hard and soft-iron parameters are estimated by performing a magnetic-alignment maneuver.  Note that the application of these corrections do not adjust individual magnetometer channels to match the actual field strength.  Only the relative magnetic field is corrected, resulting in a unit-circle for the xy magnetic-field.  However, as shown later, this enables the heading to be calculated from the corrected signal.

Magnetic-Alignment:
--------------------

A so-called “magnetic-alignment” procedure enables estimation of the hard and soft-iron disturbances in the system.  As these disturbances are fixed in the body, the corrections must be applied in the body-frame.  The procedure works as follows:

	The magnetic-field is measured and recorded as the system undergoes a 360+ degree rotation about the z-axis.  Ideally this is done when the system is level.
	Upon completion, an algorithm determines the ellipse that best fits the distorted circle.
	Ellipse parameters (related to the hard and soft-iron disturbances) are saved in the firmware and used to correct the magnetic-field measurements.
In most cases an ellipse describes magnetic-field distortions quite well.  The ellipse parameters relate to the magnetic disturbances as follows:
	The center of the ellipse is equal to the hard-iron values
	The angle the major-axis of the ellipse makes with a nominal x-axis is equal to the soft-iron angle (which forms the matrix R_{SI})
	The major and minor-axis lengths forms the scaling matrix S_{SI}
The formula for the corrected magnetic measurements works by:
	Centering the ellipse by removing the hard-iron errors from the measurements
	Rotating the ellipse to align with the nominal x and y-axes
	Stretching the ellipse to form a unit-circle
	Rotating the unit-circle back into its nominal orientation

Note: as mentioned earlier, this correction is only done in the XY-plane and cannot correct the raw magnetometer signal.  It is only done to determine the system heading.

Example:

Magnetic-field information was collected as the system underwent a 360 degree rotation about the z-axis (Figure 2).  This was performed twice, once in a disturbance-free environment (no additional iron) and once with additional iron added to the system.  The data in each case was processed and a best-fit ellipse  computed (dashed lines).  In the disturbance-free case, the data and the fit were close to circular.  In the case with additional iron, however, the circle was clearly distorted and shifted away from the origin.

Figure 2: Magnetic-Field Measurement in an Environment with and without Iron-Based Disturbances

For the measurements taken in the presence of additional iron, the estimation procedure produced the following best-fit ellipse parameters:

Table 3: Best-Fit Ellipse Parameters

Ellipse Parameter	Value	Unit
Center	-0.128, 0.126	[G]
Major/Minor axes	0.225, 0.198	[G]
Soft-Iron Scale Factor	0.882	[N/A]
Angle to Major-Axis	-48.497	[deg]

In the correction equation (above), R_{SI} is the rotation matrix and corrects for a rotation of the magnetic-field due to soft-iron effects:

R_{SI}=[■(cos(\eta)&-sin(\eta)&0@sin(\eta)&cos(\eta)&0@0&0&1)]

Where \eta is the angle from the nominal x-axis to the semi-major axis.  S_{SI} (the scale-factor matrix) corrects for the stretching caused by the soft-iron:

S_{SI}=[■(1⁄a&0&0@0&1⁄b&0@0&0&1)]

a and b are the lengths of the semi-major and semi-minor axes.

For the data-set described above, the values for R_{SI} and S_{SI}, resulting from the best-fit ellipse parameters, are:

R_{SI}=[■(0.66266&0.74892&0@-0.74892&0.66266&0@0&0&1)]

and

S_{SI}=[■(4.45226&0&0@0&5.04689&0@0&0&1)]

Applying these correction factors to the raw magnetic-field measurements results in the unit-circle shown in Figure 3.

Figure 3: Corrected Magnetic Field Readings

Note: the nodes located at 45 degree increments around the circle are points where additional data was collected to test the heading calculation (described in the next section).

Heading calculation
--------------------

The heading is computed using the fact that, in the magnetic NED-frame, the y-axis component of the magnetic field is zero.  In the true-north NED-frame this is not the case; a magnetic declination angle corrects for this.  The magnetic field at a given point can be found using the World Magnetic Model (WMM) or from NOAA’s website (https://www.ngdc.noaa.gov/geomag-web/#igrfwmm).  In San Jose, CA, the magnetic field estimates are provided in Table 4:

Table 4: Magnetic Field Components based on WMM

Figure 4 illustrates the relationship between the Lat/Lon-frame, the NED-frame, and the ⊥-frame.  Declination is specified with \delta and heading is specified with \psi.

Figure 4: Relationship of Magnetic-Field to N and B-Frames
The magnetic field vector, \vec{b}, can be broken down into two components: 1) the xy-plane component and 2) the vertical component.  The relationship between heading and magnetic field is based on the components of \vec{b}^{N} as measured in the NED-frame:
\vec{b}^{⊥}=(_^⊥)R_^N \cdot \vec{b}^{N}=(_^⊥)R_^N \cdot {■(b_{xy}@0@b_{z} )}
Expanding the expression results in the following:
{■(b_{x}^⊥@b_{y}^{⊥}@b_{z}^{⊥} )}={■(b_{xy} \cdot cos({^{N}{\psi}^{⊥}}  )@-b_{xy} \cdot sin({^{N}{\psi}^{⊥}}  )@b_{z} )}
From this, the heading is computed:
tan({^{N}{\psi}^{⊥}}  )=  (b_{xy} \cdot sin({^{N}{\psi}^{⊥}}  ))/(b_{xy} \cdot cos({^{N}{\psi}^{⊥}}  ) )=(-b_{y}^{⊥})/(b_{x}^{⊥} )=(-m_(corr,y)^⊥)/(m_(corr,x)^⊥ )
Note: the values for b_{x}^{⊥} and b_{y}^{⊥} are the corrected and ‘leveled’ values of the measured magnetic field in the body-frame, using the roll and pitch estimates to level the signal.
m ⃑_corr^⊥={^{⊥}{R}^{B}} \cdot \vec{m}_{corr}^{B}
Note: as this calculation only corrects the magnetic-field in the xy body-frame, the solution is best when the system is nearly level.  The solution begins to degrade as the roll and pitch increase.  This can be accounted for by adjusting the measurement covariance matrix, R, accordingly.  Additionally, the solution also begins to degrade as the iron in the system increases.
Results:
Table 5 lists the heading computed from test data using the above equations relating heading to corrected magnetic-field.
Table 5: Heading Results from Magnetically Clean and Distorted Readings
True Heading [deg]	Disturbance-Free Data	Data with Added Iron Sources
	Heading [deg]	Error [deg]	Heading [deg]	Error [deg]
0	359.69	-0.31	0.013	0.013
45	45.19	0.19	44.82	-0.18
90	89.96	-0.04	90.15	0.15
135	135.05	0.05	135.08	0.08
180	180.57	0.57	180.68	0.68
225	225.64	0.64	225.62	0.62
270	270.63	0.63	270.48	0.48
315	315.30	0.30	315.09	0.09
360	359.79	-0.21	0.10	0.10

Note: the raw results reported a systematic error of approximately 2.0 degrees on all heading values.  This was due to a misalignment of the test-fixture relative to true-north.  The values presented in Table 5 reflect this 2.0 degree correction.  The systematic error is visible in Figure 2 and Figure 3 with data-clusters that do not fall on the x and y-axes.
GPS Heading:
Heading is also provided directly from the GPS messages.  The four messages currently decoded by the IMU381/OpenIMU firmware provide true heading via messages listed in Table 6.
Table 6: GPS Messaging and Heading Measurement
System	Message	Description	Units
NovAtel	BESTVEL	Actual direction of motion over ground (track over ground) with respect to True North	[deg]
NMEA	VTG	True track made good	[deg]
SiRF	Geodetic Navigation Data – Message ID 41	Course Over Ground
(COG, True)	[deg x 100]
ublox	NAV-VELNED	Heading of motion 2-D	[deg]


of the PS  readings  and angles derived from accelerometer readings (equations provided in Measurement Covariance section):
GPS Position and Velocity:
GPS-based position is derived from the GPS lat/lon/alt message (BestPos, GGA, etc) and converted to NED-position using the WGS84 model.

GPS-based velocity is obtained from the BestVel, etc message.  However, the NMEA message does not provide vertical velocity, derived from or accounted for in other ways.  In all cases the N and E velocity is calculated from heading and ground speed.  The relationship is:
vN = vXY * cos(heading)
vE = vXY * sin(heading)

 
Formation of \vec{h}_{k} from EKF states:
In the measurement model (\vec{h}_{k}), all terms are functions of the system states, \vec{x}_k.  The position and velocity elements of this vector come directly from the position and velocity states, while {^{N}}{\Theta}{_{pred}^{B}} is computed from (_^N)q ⃑_pred^B , as follows:
	(_^⊥)ϕ_pred^B =atan2[2 \cdot (q_{2} \cdot q_{3}+q_{0} \cdot q_{1} ),{q_{0}}^{2}-{q_{1}}^{2}-{q_{2}}^{2}+{q_{3}}^{2} ]
	(_^⊥)θ_pred^B =-asin[2 \cdot (q_{1} \cdot q_{3}-q_{0} \cdot q_{2} )]
	{^{N}{\psi}_{pred}^{⊥}} =atan2[2 \cdot (q_{1} \cdot q_{2}+q_{0} \cdot q_{3} ),{q_{0}}^{2}+{q_{1}}^{2}-{q_{2}}^{2}-{q_{3}}^{2} ]
Observation Jacobian:
The Observation Jacobian, H, is formulated from the measurement model, \vec{h}_{k}.  The Observation Jacobian is a linearized version of the measurement model and is used to map the measurements of (_^⊥)ϕ_pred^B , (_^⊥)θ_pred^B , and {^{N}{\psi}_{pred}^{⊥}}  back to quaternion state, (_^N)q ⃑_pred^B , ensuring the EKF applies the state updates properly.  The Observation Jacobian is computed as follows:
H_k=├ (∂h ⃑)/(∂\vec{x} )┤|_(\vec{x}_k,u ⃑_k )
and results in a matrix of the form:
H_k=[■(■(I_3@0_3@0_3 )&■(0_3@I_3@0_3 )&■(■(0_(3 \times4)@0_(3 \times4)@∂h∂q)&■(0_3@0_3@0_3 )&■(0_3@0_3@0_3 )))]
where
∂h∂q=[■(H_ϕ@H_θ@H_ψ )]
The three terms that make up ∂h∂q are found using the chain-rule for differentiation.  For roll, the equation becomes:
H_ϕ=(∂(_^⊥)ϕ_pred^B )/(∂(_^N)q ⃑_pred^B  )=(∂atan2(y_ϕ,x_ϕ))/(∂x_ϕ ) \cdot (∂x_ϕ)/(∂(_^N)q ⃑_pred^B  )+(∂atan2(y_ϕ,x_ϕ))/(∂y_ϕ ) \cdot (∂y_ϕ)/(∂(_^N)q ⃑_pred^B  )
and results in:
H_ϕ=(2/(x_ϕ^{2}+y_ϕ^{2} )) \cdot [■(■((x_ϕ \cdot q_{1}-y_ϕ \cdot q_{0} )&(x_ϕ \cdot q_{0}+y_ϕ \cdot q_{1} ) )&■((x_ϕ \cdot q_{3}+y_ϕ \cdot q_{2} )&(x_ϕ \cdot q_{2}-y_ϕ \cdot q_{3} ) ))]
x_ϕ={q_{0}}^{2}-{q_{1}}^{2}-{q_{2}}^{2}+{q_{3}}^{2}
y_ϕ=2 \cdot (q_{2} \cdot q_{3}+q_{0} \cdot q_{1} )
H_ψ follows the same formulation as H_ϕ:
H_ψ=(∂{^{N}{\psi}_{pred}^{⊥}} )/(∂(_^N)q ⃑_pred^B  )=(∂atan2(y_ψ,x_ψ))/(∂x_ψ ) \cdot (∂x_ψ)/(∂(_^N)q ⃑_pred^B  )+(∂atan2(y_ψ,x_ψ))/(∂y_ψ ) \cdot (∂y_ψ)/(∂(_^N)q ⃑_pred^B  )
resulting in:
H_ψ=(2/(x_ψ^{2}+y_ψ^{2} )) \cdot [■(■(x_ψ \cdot q_{3}-y_ψ \cdot q_{0}&x_ψ \cdot q_{2}-y_ψ \cdot q_{1} )&■(x_ψ \cdot q_{1}+y_ψ \cdot q_{2}&x_ψ \cdot q_{0}+y_ψ \cdot q_{3} ))]
x_ψ= {q_{0}}^{2}+{q_{1}}^{2}-{q_{2}}^{2}-{q_{3}}^{2}
y_ψ=2 \cdot (q_{1} \cdot q_{2}+q_{0} \cdot q_{3} )
Finally, for pitch the equation becomes:
H_θ=(∂(_^⊥)θ_pred^B )/(∂(_^N)q ⃑_pred^B  )=-(∂asin(u_θ))/(∂u_θ ) \cdot (∂u_θ)/(∂(_^N)q ⃑_pred^B  )
resulting in:
H_θ=2/\sqrt{1 - u_{\theta}^{2}} \cdot [■(■(q_{2}&-q_{3} )&■(q_{0}&-q_{1} ))]
u_θ=2 \cdot (q_{1} \cdot q_{3}-q_{0} \cdot q_{2} )
Innovation (Measurement Error):
Once the measurements vectors are formed, the innovation (measurement error), ν ⃑_k, is computed:
ν ⃑_k=\vec{z}_{k}-\vec{h}_{k}
This result is used in the update stage of the EKF to generate the state error, 〖∆\vec{x}〗_k, given the Kalman gain matrix.

Magnetometer vs GPS-Heading:
These are just notes right now and may go elsewhere in the doc (probably in implementation section)
How to combine (_^N)ψ_(meas,gps)^⊥  and (_^N)ψ_(meas,mag)^⊥
	Don’t use (_^N)ψ_(meas,mag)^⊥  if (_^N)ψ_(meas,gps)^⊥  is available
	Set ν_psi=0 when GPS is valid and it is not time for a GPS update
	Create ∆(_^N)ψ_(meas,mag)^⊥  and use it for updates between GPS updates
	What if we are turning?  The latency may make the GPS heading less than ideal and affect ∆(_^N)ψ_(meas,mag)^⊥ .
	For vel < thresh, use mag, else use gps
	For vel < thresh, lock the heading update 
Measurement Covariance Values, R:
The measurement covariance is obtained in one of two ways:
	Value provided by the sensor (as for GPS messages)
	Calculated based on the underlying sensor noise
Setting this value properly is a key step toward a well-behaved EKF solution.  If the value of R is too small the Kalman gain will be large, resulting in large EKF updates.  This may work well for a static systems but will lead to errors in dynamic situations.  For example, when the Kalman gain is large, a linear acceleration in the x-axis (even for a system that has not changed attitude) can be misinterpreted as a change in the pitch.
Roll/Pitch Measurement Model and Covariance:
Static Case:
One way to determine the nominal (static) value for R is to simulate the sensor noise as it is passed through the measurement model.  For the roll and pitch angle, the models that convert the accelerometer signal to angles are simply the atan2 and asin functions.
Creating an accelerometer signal and passing it through the asin and atan2 functions reveal the noise on the measurements (during static periods), see Appendix R.  Figure 5 and Figure 6 show that the standard-deviation of the roll measurement is highly dependent on the pitch angle ({^{⊥}{\theta}^{B}} ) while the pitch standard-deviation is constant for all roll and pitch angles ().


Figure 5: Roll and Pitch Standard-Deviation due to Accelerometer Noise

Figure 6: Roll and Pitch Standard-Deviation as a function of {^{⊥}{\theta}^{B}}

In addition to finding the nominal values for R_ϕ and R_θ under level conditions ({^{⊥}{\phi}^{B}} ={^{⊥}{\theta}^{B}}=0), the change in R_ϕ for different {^{⊥}{\theta}^{B}}  should be accounted for as well.  The solution was found to become unstable (solution walked off at large pitch angles) if the change in R_ϕ vs {^{⊥}{\theta}^{B}}  was not implemented.
One final note: the values in Figure 5 and Figure 6 are standard-deviation values.  To form the R matrix, the values must be squared as R is based on the signal’s variance.
 
Heading Covariance:
The values for R_ψ can also be based on magnetometer noise levels but, if set too low, external magnetic disturbances can quickly pull the heading away from the correct value.  An empirical approach can also be used: selecting a value so sudden magnetic disturbances (such as a large truck pulling up besides the test vehicle) do not result in sudden changes in heading.  However, this can also have the negative effect that errors in the magnetic heading take some time to recover.  The second approach was taken to determine an acceptable value for R_(ψ,mag) when operating as an AHRS.
When heading is available from the GPS, this is not an issue and R_(ψ,gps) can be selected in a different manner.  As described in the BestVel GPS message description, direction accuracy is inversely proportional to vehicle speed.  The faster the system is traveling, the better the heading measurement.  This relationship can be used to set R_(ψ,gps).
At slow speeds (or a stop), R_(ψ,gps) will get very large.  Two approaches to deal with these cases are to
	Implement a yaw-lock.  Prevent a yaw update during these periods.
	Use the magnetometer solution at speeds below a certain threshold.
Dynamic Case:
To find the appropriate R-values, a Monte-Carlo approach was used.  For the …

Aided VG-Solution


Implementation
One of the challenges in implementing the Extended Kalman Filter comes from determining the quality of the measurement and setting the measurement covariance, R, appropriately.  As mentioned previously, roll and pitch measurements are nominally computed from static accelerometer noise levels.  However, when the system is moving, the accelerometer signal may also contains linear and centripetal acceleration components (as well as system vibrations).  These components distort the gravity measurement and affect the roll and pitch estimates as the system does not know if the measured angles are changing due to a change in attitude (gravity) or a linear acceleration.
In practice, discerning between the gravity and motion (and adjusting R accordingly) has the potential to improve the attitude results.  In this case, adjusting the value of R during acceleration periods (increasing the value) reduces the effect of the acceleration on the state update.  When the system returns to a static (non-accelerating) state, the value of R can be reduced to the nominal value, which results in a higher Kalman gain  and more aggressive updates.
A simple approach to implementing this is to compare the magnitude of the accelerometer signal against the expected magnitude of gravity.  When an appreciable difference is detected (more than typical sensor/system noise would cause), the value of R is increased.  When the difference is removed, the value of R is restored.  While simple in theory, this is more difficult in practice.  Why?  To avoid single point errors (mitigated by using the signal only after a certain amount of time elapses).  To ensure the gain drops before the measurement is used (filter properly).
Other things to improve performance:
	Limit the innovation error, ν ⃑_k.  This reduces the error going into the EKF Update resulting in smaller state updates.  Setting the error limit this way is justified as the errors are typically only large during periods of acceleration, which are erroneous anyway.
	Change R based on the quality of the measurement.  Some measurements (particularly GPS measurements) are provided along with a measure of their variance.  When available, these values can be used to adjust R.  Other measurements do not provide this information and the user is left to set R based on intuition or simulation.  For instance, as mentioned above, ϕ and θ are affected by acceleration; R_ϕ and R_θ should be increased during these periods.  ψ is affected by turns about the z-axis and R_ψ should be increased accordingly to account for lag and other effects.
	Combining heading from two sources
	Need to think of how to combine these two measurements
	Don’t use mag heading when GPS valid?
	Latency in GPS message: Any latency in obtaining, parsing, and providing GPS messages should be accounted for by either 1) adjusting R or 2) accounting for the latency.  For instance, if the GPS messages is consistently late by DT seconds, then the heading can be adjusted by a formula such as:
ψ_GPS=ψ_GPS- ψ ̇ \cdot DT

	Much of the math on which the EKF is based is represented by sparse matrices.  Using mathematical operations that are based on sparse matrices make the algorithms run much faster and permit higher execution rates.  For the most part, only the P-matrix needs to have all its elements considered.
	The INS algorithm makes use of a sequential approach to solving for the states.  From an execution point-of-view this makes the runtime of the algorithm significantly less as only 3x3 matrix inverses are required to solve for the state updates

 
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
〖w_q \cdot 〖w_q〗^T=(Δt/2)〗^{2} \cdot (Ξ \cdot Σ_ω \cdot Ξ^T )
The rate-sensor noise is treated as a stationary process, so the time subscript, k, can be dropped from the noise terms.  However, the attitude does change with time and k should remain on the quaternion terms (removed here for ease of reading).  Additionally, the sensor noise is assumed to be the same for all sensor channels.
Ξ≡[■(-〖\vec{q}_{v}〗^T@q_{0} \cdot I_3+[\vec{q}_{v} \times] )]
〖w_q \cdot 〖w_q〗^T=(Δt/2)〗^{2} \cdot [■(■(-q_{1}&-q_{2}@q_{0}&-q_{3} )&■(-q_{3}@q_{2} )@■(q_{3}&q_{0}@-q_{2}&q_{1} )&■(-q_{1}@q_{0} ))] \cdot [■(〖σ_ω〗^{2}&0&0@0&〖σ_ω〗^{2}&0@0&0&〖σ_ω〗^{2} )] \cdot [■(■(-q_{1}&q_{0} )&■(q_{3}&-q_{2} )@■(-q_{2}&-q_{3} )&■(q_{0}&q_{1} )@■(-q_{3}&q_{2} )&■(-q_{1}&q_{0} ))]
〖w_q \cdot 〖w_q〗^T=(Δt/2)〗^{2} \cdot 〖σ_ω〗^{2} \cdot [■(■(-q_{1}&-q_{2}@q_{0}&-q_{3} )&■(-q_{3}@q_{2} )@■(q_{3}&q_{0}@-q_{2}&q_{1} )&■(-q_{1}@q_{0} ))] \cdot [■(■(-q_{1}&q_{0} )&■(q_{3}&-q_{2} )@■(-q_{2}&-q_{3} )&■(q_{0}&q_{1} )@■(-q_{3}&q_{2} )&■(-q_{1}&q_{0} ))]
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
