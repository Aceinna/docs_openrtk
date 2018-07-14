**************
Kalman Filter
**************

.. contents:: Contents
    :local:

.. role::  raw-html(raw)
    :format: html
    
.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>

	
The solution described in this document is based on a Kalman Filter that generates estimates of
attitude, position, and velocity from noisy sensor readings.  In particular, an Extended Kalman
Filter (EKF) is used due to the nonlinear nature of the process and measurements models.


The Kalman filter operates on a predict/update cycle\ [#EKF_Ref]_.  The system state at the next
time step is estimated from current states and system inputs.  For attitude calculations, this
input is the angular rate-sensor signal; velocity and position calculations use the
accelerometer as an input.  The update stage corrects the state estimates for errors inherent in
the rate-sensor signal (such as sensor bias and drift) using measurements of the true attitude
derived from the accelerometer, magnetometer, and GPS readings.  As these signals are typically
noisier\ [#EKF_Noisier]_ or provided at a significantly lower rate than the rate-sensor, they are
not used to propagate the attitude, instead their information is used to correct the errors in
the estimate.


For a discrete-time system the prediction and update equations are:

Prediction (High DR)
=====================

In this stage of the EKF, the attitude, velocity, and acceleration are propagated forward in time
from sensor readings.

.. math::

    \vec{x}_{k|k-1} = f\begin{pmatrix} {\vec{x}_{k-1|k-1}, \vec{u}_{k|k-1}} \end{pmatrix}

.. math::

    P_{k|k-1} = F_{k-1} \cdot P_{k-1|k-1} \cdot {F_{k-1} }^{T} + Q_{k-1}
    

The first equation (:math:`\vec{x}_{k|k-1}`) is the State Prediction Model and the second
(:math:`P_{k|k-1}`) is the Covariance Estimate.


Innovation (Measurement Error)
===============================

In this stage, the errors between the predicted states and the measurements are computed.

.. math::

    \vec{\nu}_{k} = \vec{z}_{k} - \vec{h}_{k}


Update (Low DR)
================

The final stage of the EKF generates updates (corrections) to the predictions based on the quality
of the process models, process inputs, and measurements.

.. math::

    \begin{aligned}
    {
    S_{k} = H_{k} \cdot P_{k|k-1} \cdot {H_{k} }^{T} + R_{k}\\
    {\hspace{5mm}} \\
    K_{k} = P_{k|k-1} \cdot {H_{k} }^{T} \cdot  {S_{k}}^{-1}\\
    {\hspace{5mm}} \\
    \Delta{\vec{x}_{k}} = K_{k} \cdot \vec{\nu}_{k}\\
    {\hspace{5mm}} \\
    \vec{x}_{k|k} = \vec{x}_{k|k-1} + \Delta{\vec{x}_{k}}\\
    {\hspace{5mm}} \\
    \Delta{P_{k}} = -K_{k} \cdot H_{k} \cdot P_{k|k-1}\\
    {\hspace{5mm}} \\
    P_{k|k} = P_{k|k-1} + \Delta{P_{k}}
    }
    \end{aligned}


In order, the above equations relate to the

    1. Innovation Covariance
    2. Kalman Gain
    3. State Update
    4. Updated State
    5. Covariance Update
    6. Updated Covariance


These terms will be defined in the sections that follow.


.. [#EKF_Ref] Kalman Filtering: Theory and Practice Using MATLAB, 3rd Edition, Mohinder S. Grewal,
              Angus P. Andrews

.. [#EKF_Noisier] In this case, noisier means that the sensor signals are corrupted, not just by
                  electrical noise, but by external influences as well.  In the case of the
                  accelerometer, the device picks up vehicle motion in addition to gravity
                  information.  The magnetometer signal is affected by external magnetic sources,
                  such as iron in passing vehicles and in roadways.

