:tocdepth: 3


Position State-Transition Model
**********************************


The position process model is based on the following first-order model:

.. math::

    \vec{r}_{k} = \vec{r}_{k-1} + \dot{\vec{r}}_{k-1} \cdot dt


where :math:`\dot{\vec{r}}_{k-1}` is equal to the estimated velocity state, :math:`\vec{v}_{k-1}`.
Substituting in the velocity term (including noise) results in:

.. math::

    \vec{r}_{k} = \vec{r}_{k-1} + \vec{v}_{k-1} \cdot dt + \vec{w}_{r,k-1}


:math:`\vec{w}_{r,k-1}` is the process noise associated with the position state-transition model,
which is directly related to the velocity process noise:

.. math::

    \vec{w}_{r,k-1}	&= {\vec{w}_{v,k-1}} \cdot dt\\
                    {\hspace{5mm}} \\
                    &= {^{N}{R}_{k-1}^{B}} \cdot {\vec{a}_{noise}^{B}} \cdot {dt}^{2}


Like the previous process models, this expression is used to compute the elements of the process
covariance matrix (Q) related to the position estimate:

.. math::

    \Sigma_{r} = {\vec{w}_{r,k-1}} \cdot {\vec{w}_{r,k-1}}^{T}


By making the assumption that all axes have the same noise characteristics
(:math:`{\sigma_{a}}^{2}`), :math:`\Sigma_{r}` simplifies to:

.. math::

    \Sigma_{r} = ({\sigma_{a} \cdot dt}^{2} )^{2} \cdot I_3


See Appendix Q for further information.
