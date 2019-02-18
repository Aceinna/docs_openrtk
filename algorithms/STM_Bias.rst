:tocdepth: 3


Rate and Acceleration Bias State-Transition Models
---------------------------------------------------

.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


The process models for the bias terms are based on the assumption that bias is made up of two
components:

    1) A **constant** bias offset (:math:`\vec{\omega}_{offset}^{B}`)

    2) A **randomly varying** component superimposed on the offset
       (:math:`\vec{\omega}_{drift}^{B}`) based on the measured bias-instability value of the sensor


For the rate-sensor, the bias model is

.. math::

    \vec{\omega}_{bias}^{B} = \vec{\omega}_{offset}^{B} + \vec{\omega}_{drift}^{B}


The drift model follows a random-walk process\ [#RW_Assump]_, i.e. the drift value wanders according
to a Gaussian distribution.

.. math::

    \vec{\omega}_{drift,k}^{B} = \vec{\omega}_{drift,k-1}^{B} + \dot{\vec{\omega}}_{drift,k-1}^{B} \cdot dt


where

.. math::

    \dot{\vec{\omega}}_{drift,k-1}^{B} = N \begin{pmatrix} { 0,\sigma_{dd,\omega}^{2} } \end{pmatrix}


.. note::

    The subscript *dd* stands for "drift-dot".

Based on this model, the process variance for :math:`\vec{\omega}_{drift}^{B}` at time, t, is given
by:

.. math::

    \sigma_{d,\omega}^{2}(t) = \begin{bmatrix} { (\sigma_{dd,\omega} \cdot \sqrt{dt}) \cdot \sqrt{t} } \end{bmatrix} ^{2}


An empirical study related :math:`\sigma_{dd,\omega}` to the BI and ARW values as follows:

.. math::

    \sigma_{dd,\omega} = {{2 \cdot \pi} \over {ln(2)}} \cdot {{{BI}^{2}} \over {ARW}}


To find the rate-bias process-noise covariance, set :math:`t = dt` in the process-variance model
(above), resulting in:

.. math::

    \Sigma_{\omega b} = \sigma_{d,\omega}^{2} (dt) \cdot I_3 = {\begin{pmatrix} { \sigma_{dd,\omega} \cdot dt } \end{pmatrix}}^{2} \cdot I_3


The accelerometer drift model mirrors this formulation and results in:

.. math::

    \Sigma_{ab} = \sigma_{d,a}^{2} (dt) \cdot I_3 = {\begin{pmatrix} { \sigma_{dd,a} \cdot dt } \end{pmatrix}}^{2} \cdot I_3


.. [#RW_Assump] This is not a perfect assumption as the output of the model is unbounded while the
                actual process is not.
