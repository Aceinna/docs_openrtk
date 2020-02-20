RTK Robust Extended Kalman Filter
=================================

Robust extended kalman filter is an optimal autoregressive algorithm for solving discrete data. 
It takes the observed signal as the parameter of the state equation, and the observed noise is 
Gaussian white noise, and calculates predicted value. EKF is a recursive filtering method which proposed
base on probability theory and mathematical statistics. This method has a small calculation and powerful 
functions, and is convenient for processing real-time data. It is widely used in satellite navigation and 
positioning, aircraft control, and other fields.

State models
------------

.. math::

 X_{k,k-1}=\Phi_{k,k-1}X_{k-1}+\Gamma_kW_k

Where: :math:`X_{k,k-1}` is a vector representing the parameter to be estimated at :math:`k` time; 
:math:`\Phi_{k,k-1}` is a state transition matrix representing the state of the system from :math:`k-1` time to :math:`k` time;
which is a non-singular unit matrix; :math:`\Gamma_k` is a noise drive matrix and :math:`W_k` is a system noise matrix.

Observation models
-----------------

.. math::

 L_k = H_kX_k + Z_k

Where::math:`H_k` is a coefficient matrix of observed equations; :math:`L_k` is a vector of observations;
:math:`Z_k` representing a residual of observed equations . 

When state systems and observation models are used to describe the system, it is usually assumed 
that the observed values are independent of each other, the system noise and the observed noise are 
independent of each other, and obey the Gaussian white noise with zero mean, that is,

.. math::
  
  \left\{
  \begin{aligned}
  & E\{U_k\} = E\{V_k\} = E\{U_k \cdot V_j^T\} = 0 \\
  & E\{U_k \cdot U_j^T\} = \begin{cases} Q_k, j=k \\ 0, j \ne k\end{cases}\\
  & E\{V_k \cdot V_j^T\} = \begin{cases} R_k, j=k \\ 0, j \ne k\end{cases}
  \end{aligned}
  \right.

The Kalman filter solution process is divided into prediction process, filtering gain, and estimated 
value calculation. The calculation is as follows:

1. Prediction process
---------------------

Firstly, Calculating the current predicted value based on the filtered value at the previous epoch:

.. math::

 X_{k,k-1} = \Phi_{k,k-1}X_{k-1}

Calculating the prediction values based on the error variance matrix and Gaussian white noise with 
the previous epoch:   

.. math::

 P_{k,k-1} = \Phi_{k,k-1}P_{k-1}\Phi_{k,k-1}^T + Τ_kQ_kT_k^T

2. Calculating the filter gain
------------------------------

The so-called filter gain is to calculate the weight of the estimated variance in the total variance 
(estimated variance and observed variance). The formula is as follows:

.. math::

 K_k = P_{k,k-1}H_k^T(H_kP_{k,k-1}H_k^T +R_k)^{-1}

3. Calculating the filter estimates
-------------------------------

Calculating filter estimate values of :math:`k` epoch:

.. math::

 X_k = X_{k,k-1} + K_k(L_k - H_kX_{k,k-1})

Calculating the error matrix of the filter estimate values:

.. math::

 P_k = P_{k,k-1}(E-K_kH_k)

Where: :math:`E` Represents the unit matrix. 

From the above process, it can be seen that Kalman filter is a process of continuous prediction and 
correction. Each calculation only needs the observation value at the last epoch. The calculation is 
simple and does not need to store a large amount of data, which is conducive to real-time estimation.

Extended Kalman Filter Model in Relative Positioning
----------------------------------------------------

Because the original observations include three types of observations: pseudorange, carrier, and 
doppler shift, filtering and solving the three observations separately can greatly reduce the 
dimension of the matrix, reducing the amount of calculation and the calculation space.

Updating the observation equation to obtain the updated position velocity and time information as 
initial values, and then perform observation updates.

First, filter calculations are performed on the pseudorange. The observation equation is as follows:

.. math::

 Z_k = L_k - H_kX_k

Where:

.. math::

 Z_k = \Delta P - (\Delta \rho + \Delta tro)

The :math:`\Delta P` matrix represents the single-difference pseudorange observations between rover station 
and base station; the :math:`\Delta \rho` matrix represents the initial single-difference results of 
geometric-range; and the :math:`\Delta tro` represents the single-difference values of tropospheric.

The coefficient matrix is:

.. math::

 H_k = \begin{bmatrix}\partial{X} & \partial{Y} & \partial{Z} & O & O & O & O & O & O & O & I & E \end{bmatrix}

Where: :math:`\partial X` is the unit vector of geometric-range in the :math:`X` direction; :math:`\partial Y` is the 
unit vector of geometric-range in the :math:`Y` direction; :math:`\partial Z` is the unit vector of geometric-range 
in the :math:`Z` direction; :math:`I` is the unit column vector; and :math:`E` is the unit matrix which can be calculated 
according to the system of observations and the frequency.

Parameters to be evaluated:

.. math::

 X_k = \begin{bmatrix}dX & dY & dZ & dV_X & dV_Y & dV_Z & da_X & da_Y & da_Z & cdt_r & cdt_{bias}\end{bmatrix}

Where: :math:`dX, dY, dZ` distribution indicates the correction values of :math:`X`, :math:`Y`, and :math:`Z` axes; 
:math:`dV_X`, :math:`dV_Y`, :math:`dV_Z` is corresponding speed correction value; and :math:`da_X`, :math:`da_Y`, :math:`da_Z`
is corresponding acceleration correction value. :math:`cdt_r` is the clock drift; :math:`cdt_{bias}` is the clock error bias matrix. 
Different systems and different frequency parameters are inconsistent, The first parameter is the 
single-difference clock error of GPS L1.

Furthermore, filtering calculation is performed on the doppler observation equation, and the 
parameter :math:`X_k` to be estimated is the same as the pseudorange observation equation

.. math::

 Z_k = \lambda D - (\dot \rho - c{\dot{t}}^j)

In the formula, :math:`D` is the doppler frequency observation matrix, :math:`\lambda` is the corresponding 
carrier wavelength; :math:`\dot \rho` is the rate of change of geometric-range; :math:`c` is the speed of light;
and :math:`{\dot{t}}^j` is the clock drift of the satellite.

The coefficient matrix is:

.. math::

 H_k = \begin{bmatrix}& O & O & O & \partial X & \partial Y & \partial Z & O & O & O & I\end{bmatrix}

Finally, calculating the carrier observation equation. In addition to the parameters in the 
pseudorange model, the parameters to be estimated also added ambiguity parameters. The ambiguity 
parameters are divided into two parts. The first part is the single-difference ambiguity :math:`\Delta N_{ref}`
of the reference satellite. The second part is the double-difference ambiguity of the observation 
satellite :math:`\Delta \nabla N_{rov}`. The expression of the parameter to be evaluated is as follows:

.. math::

 X_k = \begin{bmatrix}dX & dY & dZ & dV_X & dV_Y & dV_Z & da_X & da_Y & da_Z & cdt_r & cdt_{bias} & \Delta N_{ref} & \Delta \nabla N_{rov} \end{bmatrix}

The carrier observation equation is:

.. math::

 Z_k = \Delta \phi - (\Delta \rho + \Delta tro + ct_i) / \lambda
    
In the formula, :math:`\Delta \phi` represents the single difference observation value of carrier; and
:math:`ct_i` represents the single difference clock error of the receiver, which is calculated according 
to :math:`cdt_{bias}`.

For observation satellites, the single-difference ambiguity:

.. math::

 \Delta N_{rov} = \Delta N_{rov} - \Delta N_{ref} + \Delta N_{ref} = \Delta \nabla N_{rov} + \Delta N_{ref}

Therefore, the corresponding coefficient matrix is:

.. math::

 H_k = \begin{bmatrix}\partial X & \partial Y & \partial Z & O & O & O & O & O & O & O & O & E & O \\
 \partial X & \partial Y & \partial Z & O & O & O & O & O & O & O & O & E & E\\ \end{bmatrix}

According to the above three sets of observations, the observations are updated separately, and 
finally the high-precision position velocity and time information are calculated.

