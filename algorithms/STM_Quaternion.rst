:tocdepth: 3


Quaternion State-Transition Model
**********************************


All state propagation equations used in this paper are based on the following Taylor-series
expansion:

.. math::

    \vec{x}_{k} = \vec{x}_{k-1} + \dot{\vec{x}}_{k-1} \cdot { {dt} \over {1!} } + \ddot{\vec{x}}_{k-1} \cdot { {dt}^2 \over {2!} } + \ldots


where terms higher than first-order are neglected.  For attitude, the quaternion is propagated
according to the expression:

.. math::

    \vec{q}_{k} \approx \vec{q}_{k-1} + \dot{\vec{q}}_{k-1} \cdot dt


The kinematical equation that describes the rate-of-change of the attitude quaternion,
:math:`\dot{\vec{q}}_{k-1}`, is a function of **true angular velocity**,
:math:`\vec{\omega}_{true}`, as follows:

.. math::

    \dot{\vec{q}}_{k-1} = { {1} \over {2} } \cdot \Omega_{true,k-1} \cdot \vec{{q}}_{k-1}


where :math:`\Omega_{true,k-1}` is formed from the components of the angular rate vector,
:math:`{\begin{pmatrix}{^{N}{\vec{\omega}_{true}}^{B}}\end{pmatrix}}^{B}` and specifies the angular-
rate of the body relative to an inertially-fixed frame, measured in the body-frame.  As all angular-
rate measurements made with MEMS sensors are relative to the inertial-frame, the notation is
simplified to :math:`{\vec{\omega}_{true}}^{B}`.

.. math::

    \vec{\omega}^{B} = { \begin{Bmatrix} { \omega_{x}^{B} \\
                                           \omega_{y}^{B} \\
                                           \omega_{z}^{B}
                         } \end{Bmatrix}
                       }


The quaternion propagation matrix, :math:`\Omega_{k-1}`, at time-step k-1 is:

.. math::

    \Omega_{k-1} = { \begin{bmatrix} { { 0 \\
                                         \omega_{x,k-1}^{B} \\
                                         \omega_{y,k-1}^{B} \\
                                         \omega_{z,k-1}^{B}
                                       } \hspace{5mm}
                                       { -\omega_{x,k-1}^{B} \\
                                         0 \\
                                         -\omega_{z,k-1}^{B} \\
                                         \omega_{y,k-1}^{B}
                                       } \hspace{5mm}
									   { -\omega_{y,k-1}^{B} \\
                                         \omega_{z,k-1}^{B} \\
                                         0 \\
                                         -\omega_{x,k-1}^{B}
                                       } \hspace{5mm}
									   { -\omega_{z,k-1}^{B} \\
                                         -\omega_{y,k-1}^{B} \\
                                         \omega_{x,k-1}^{B} \\
                                         0
                                         }
                     } \end{bmatrix}
                   }


where (as noted above) all the rate components are estimates of the “true” rate measurements.


From the above expressions, the full state-transition model for system-attitude is:

.. math::

    \vec{q}_{k} = \vec{q}_{k-1} + {{1} \over {2}} \cdot \Omega_{true,k-1} \cdot {\vec{q}}_{k-1} \cdot dt
                = { \begin{bmatrix} {
                                      I_4 + {{dt} \over {2}} \cdot \Omega_{true,k-1}
                    } \end{bmatrix}
                  } \cdot {\vec{q}}_{k-1}


where *dt* is the integration time-step (sampling interval) and :math:`\vec{q}_{k-1}` is the
current estimate of system attitude.


To find the noise term in the state-transition model, :math:`\vec{w}_{q,k-1}`, expand the
expression for :math:`\Omega_{true,k-1}` using the rate-sensor model described earlier to
explicitly show the constituent terms:

.. math::

    \Omega_{true,k-1} = \Omega_{meas,k-1} - \Omega_{bias,k-1} - \Omega_{noise,k-1}


Substitute this result into the expression for the attitude state-transition model:

.. math::

    \vec{q}_{k} = { { \begin{bmatrix} {
                        I_4 + {{dt} \over {2}} \cdot \begin{pmatrix} { \Omega_{meas,k-1} - \Omega_{bias,k-1} } \end{pmatrix}
                            - {{dt} \over {2}} \cdot \Omega_{noise,k-1}
                    } \end{bmatrix}
                  } \cdot {\vec{q}}_{k-1} }


.. math::

    = { \Phi_{k-1} \cdot \vec{q}_{k-1} + \vec{w}_{q,k-1} }


:math:`\Phi_{k-1}` is the state-transition matrix, defined as:

.. math::

    \Phi_{k-1} \equiv I_4 + {{dt} \over {2}} \cdot \begin{pmatrix} { \Omega_{meas,k-1} - \Omega_{bias,k-1} } \end{pmatrix}


and :math:`\vec{w}_{q,k-1}` is the quaternion process-noise vector:

.. math::

    \vec{w}_{q,k-1} = -{{dt} \over {2}} \cdot \Omega_{noise,k-1} \cdot \vec{q}_{k-1}


Note: In this expression, the components of :math:`\Omega_{noise}` are the noise components of
:math:`\vec{\omega}^{B}` (:math:`\sigma_{\omega}^{2}`) which can be expressed in terms of the
sensor’s ARW.


Recasting :math:`\vec{w}_{q,k-1}`, so the rate-sensor noise (:math:`\omega_{noise}^{B}`) forms
the input vector (Appendix Q), results in the final expression for the quaternion process-noise
resulting from rate-sensor noise:

.. math::

    \vec{w}_{q,k-1} = -{{dt} \over {2}} \cdot \Xi_{k-1} \cdot \vec{\omega}_{noise}^{B}


with the variable :math:`\Xi_{k-1}` relating the change in process noise to system attitude

.. math::

    \Xi_{k-1} \equiv \begin{bmatrix} { {-\vec{q}_{v}^{T}} \\
                                       {q_0 \cdot I_3 + \begin{bmatrix} {\vec{q}_{v} \times} \end{bmatrix}}
                     } \end{bmatrix}


and :math:`\begin{bmatrix} {\vec{q}_{v} \times} \end{bmatrix}` is the cross-product matrix described
in Appendix A.


The quaternion process noise vector is used to form the elements of the process covariance
matrix (Q) related to the attitude state.  The covariance is computed according to the following
equation:

.. math::

    \Sigma_{ij} = cov \begin{pmatrix} {\vec{x}_{i}, \vec{x}_{j}} \end{pmatrix}
                = E \begin{bmatrix} {\begin{pmatrix} {\vec{x}_{i} - \mu_i} \end{pmatrix}
                                     \cdot
                                     \begin{pmatrix} {\vec{x}_{i} - \mu_j} \end{pmatrix}
                    } \end{bmatrix}


As mentioned previously, all processes considered in this paper assume white (zero mean) sensor
noise that is uncorrelated across sensor channels.  This simplifies the expression for the
covariance to:

.. math::

    \Sigma_{q} = \vec{w}_{q,k-1} \cdot \vec{w}_{q,k-1}^{T}


In addition to the assumption that the noise terms are white and independent, all axes are assumed
to have the same noise characteristics (:math:`\sigma_{\omega}`).  Resulting in the final expression
for :math:`\Sigma_{q}` (Appendix Q):

.. math::

    \Sigma_{q} = { { \begin{pmatrix} {
                                       {\sigma_{\omega} \cdot dt } \over {2}
                     } \end{pmatrix} }^{2}
                 }
                 \cdot
                 { \begin{bmatrix} { {  {1 - q_0^2}\\
                                       -{q_0 \cdot q_1}\\
                                       -{q_0 \cdot q_2}\\
                                       -{q_0 \cdot q_3}
                                     } \hspace{5mm}
                                     { -{q_0 \cdot q_1}\\
                                        {1 - q_1^2}\\
                                       -{q_1 \cdot q_2}\\
                                       -{q_1 \cdot q_3}
                                     } \hspace{5mm}
    								                 { -{q_0 \cdot q_2}\\
                                       -{q_1 \cdot q_2}\\
                                        {1 - q_2^2}\\
                                       -{q_2 \cdot q_3}
                                     } \hspace{5mm}
    								                 { -{q_0 \cdot q_3}\\
                                       -{q_1 \cdot q_3}\\
                                       -{q_2 \cdot q_3}\\
                                        {1 - q_3^2}
                                     }
                    } \end{bmatrix}
                  }
