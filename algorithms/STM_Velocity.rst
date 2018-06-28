:tocdepth: 3


Velocity State-Transition Model
**********************************


The velocity propagation equation is based on the following first-order model:

.. math::

    \vec{v}_{k} = \vec{v}_{k-1} + \dot{\vec{v}}_{k-1} \cdot dt

:math:`\dot{\vec{v}}_{k-1}` is an estimate of system motion (linear-acceleration corrected for
gravity) and is formed from the accelerometer signal with estimated accelerometer-bias and gravity
removed.

.. math::

    \vec{a}_{motion,k-1} = \vec{a}_{meas,k-1} - \vec{a}_{bias,k-1} - \vec{a}_{grav}


Substituting this expression (along with the noise term) into the velocity propagation equation, and
explicitly stating the frames in which the signals are available, leads to:

.. math::

    \vec{v}_{k}^N = \vec{v}_{k-1}^N + \begin{pmatrix} {
                                                        \vec{a}_{motion,k-1}^N - {^{N}{R}_{k-1}^{B}} \cdot \vec{a}_{noise}^{B}
                                      } \end{pmatrix} \cdot {dt}


where

.. math::

    \vec{a}_{motion,k-1}^N = {^{N}{R}_{k-1}^{B}} \cdot \begin{pmatrix} {
                                                                         \vec{a}_{meas,k-1}^B - \hat{a}_{bias,k-1}^B
                                                        } \end{pmatrix} - \vec{a}_{grav}^{N}


The velocity process-noise vector resulting from accelerometer noise is:

.. math::

    \vec{w}_{v,k-1}^{N} = -{^{N}{R}_{k-1}^{B}} \cdot \vec{a}_{noise}^{B} \cdot {dt}


leading to the final formulation for the velocity state-transition model:

.. math::

    \vec{v}_{k}^N = \vec{v}_{k-1}^N + \vec{a}_{motion,k-1}^N \cdot dt + \vec{w}_{v,k-1}^{N}


The velocity process noise vector is used to compute the elements of the process covariance matrix
(:math:`Q`) related to the velocity estimate, as follows:

.. math::

    \Sigma_{v} = {\vec{w}_{v,k-1}} \cdot {\vec{w}_{v,k-1}}^{T}

By making the assumption that all axes have the same noise characteristics
(:math:`{\sigma_{a}}^{2}`) and manipulating the expression, the result can be simplified to the
following (Appendix Q):

.. math::

    \Sigma_{v} = { \begin{pmatrix} {
                                     \sigma_{a} \cdot dt
                   } \end{pmatrix} }^{2} \cdot I_3
