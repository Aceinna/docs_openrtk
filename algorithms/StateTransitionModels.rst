************************
State Transition Models
************************


.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


System State-Transition Model Summary\ [#Algo_Ref]_
====================================================

The state-transition models form the core of the EKF prediction stage by performing the following
roles:

    1) They form the equations that propagate the system states from one time-step to the next
       (using high-quality sensor as the input)

    2) They define the process-noise vectors relating each state to sensor noise

    3) They enable computation of the process covariance matrix, Q, and process Jacobian, F.  Both
       are used to propagate the system covariance, P, from one time-step to the next.


The complete system state equation consists of 16 total states\ [#LLA_Conv]_


.. math::
    \vec{x} = {
                \begin{Bmatrix} {
                                  \begin{array}{c}
                                                   {\vec{r}^{N}} \\
                                                   {\vec{v}^{N}} \\
                                                   {{^N}\vec{q}{^B}} \\
                                                   {\vec{\omega}_{bias}^{B}} \\
                                                   {\vec{a}_{bias}^{B}}
                                  \end{array}
                } \end{Bmatrix}
              }
            = {
                \begin{Bmatrix} {
                                  \begin{array}{c}
                                                   {\text{NED Position (3)}} \\
                                                   {\text{NED Velocity (3)}} \\
                                                   {\text{Body Attitude (4)}} \\
                                                   {\text{Angular-Rate Bias (3)}} \\
                                                   {\text{Accelerometer Bias (3)}}
                                  \end{array}
                } \end{Bmatrix}
              }


with the state-transition model, :math:`\vec{f}`, made up of five individual models (developed
in upcoming sections):

.. math::

    \vec{x}_{k} = \vec{f} { \begin{pmatrix} {
                                              \vec{x}_{k-1}, \hspace{2mm}
                                              \vec{u}_{k-1}
                                            }
                            \end{pmatrix} } + \vec{w}_{k-1}


where :math:`\vec{x}` is the state-vector, :math:`\vec{u}` is the input-vector (consisting of sensor
signals) and :math:`\vec{w}` is the process-noise vector.

The expanded state-transition vector, :math:`\vec{f}`, is:

.. math::

    \vec{f} { \begin{pmatrix} {
                                \vec{x}_{k-1}, \hspace{2mm}
                                \vec{u}_{k-1}
              } \end{pmatrix} } = { \begin{Bmatrix} {
                                                      \begin{array}{c}
                                                                       {\vec{r}_{k-1}^{N} + \vec{v}_{k-1}^{N} \cdot dt} \\
                                                                       {\vec{v}_{k-1}^{N} + \begin{bmatrix} {
                                                                                                             {{{^N}{R}_{k-1}^{B}} \cdot \begin{pmatrix} {
                                                                                                                                        \vec{a}_{meas,k-1}^{B} - \hat{a}_{bias,k-1}^{B}
                                                                                                                        } \end{pmatrix} - \vec{a}_{grav,k-1}^{N}
                                                                                                             }
                                                                                            } \end{bmatrix}  \cdot dt
                                                                       } \\
                                                                       { \begin{bmatrix} {
                                                                                           I_4 + {{dt} \over {2}} \cdot \begin{pmatrix} { \Omega_{meas,k-1} - \Omega_{bias,k-1}
                                                                                                   } \end{pmatrix}
                                                                         } \end{bmatrix} \cdot {^N}\vec{q}_{k-1}^{B}
                                                                       } \\
                                                                       {I_3} \\
                                                                       {I_3}
                                                      \end{array}
                                    } \end{Bmatrix}
              }


and the process-noise vector, :math:`\vec{w}_{k-1}`, is:

.. math::

    \vec{w}_{k-1} = { \begin{Bmatrix} {
                                        \begin{array}{c}
                                                         {-{^{N}{R}_{k-1}^{B}} \cdot \vec{a}_{noise}^{B} \cdot {dt}^{2}} \\
                                                         {-{^{N}{R}_{k-1}^{B}} \cdot \vec{a}_{noise}^{B} \cdot {dt}} \\
                                                         {-{{dt} \over {2}} \cdot \Xi_{k-1} \cdot {\vec{w}_{noise}^{B}}} \\
                                                         { \vec{N} \begin{pmatrix} {
                                                                                     0, \hspace{1mm}
                                                                                     \sigma_{dd,\omega}^{2}
                                                                   } \end{pmatrix} } \\
                                                         { \vec{N} \begin{pmatrix} {
                                                                                     0, \hspace{1mm}
                                                                                     \sigma_{dd,a}^{2}
                                                                   } \end{pmatrix} }
                                        \end{array}
                      } \end{Bmatrix}
                    }


The sensor noise vectors, :math:`\vec{N}`, corresponding to the angular-rate and accelerometer bias
states, are each 3x1 vectors with elements described by a zero-mean Gaussian distribution with a
variance of either :math:`\sigma_{dd,\omega}^{2}` or :math:`\sigma_{dd,a}^{2}`\ .


.. contents:: Contents
    :local:

.. role::  raw-html(raw)
    :format: html


Individual State-Transition Models
===================================

Individual state-transition models are derived in the following sections:

.. toctree::
    :maxdepth: 3

    STM_Quaternion
    STM_Velocity
    STM_Position
    STM_Bias


.. links-placeholder




.. [#Algo_Ref] There are many papers describing the derivation and implementation issues for EKFs
               and Complementary-Filters.  Several of the papers similar to this implementation are
               referenced in the *Reference* section.

.. [#LLA_Conv] GPS measurements are in latitude/longitude/altitude.  These are converted to position
               in the Earth-frame, :math:`\vec{r}{^E}`.  Position in the NED-frame is calculated
               from the initial starting point at system startup.  The state estimate is generated
               by integrating velocity (estimated from accelerometer data).
