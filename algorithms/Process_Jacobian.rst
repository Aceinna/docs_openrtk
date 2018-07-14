:tocdepth: 3


Process Jacobian
-----------------

.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


As the system is nonlinear, the vector :math:`\vec{f}` cannot be used to propagate the covariance
matrix, :math:`P`.  Instead the Process Jacobian, :math:`F`, (a linearized version of the state-
transition vector) is computed at each time step (based on the current system states) to propagate
:math:`P` forward in time:


.. math::

    F_{k-1} = \left.{ {\partial{\vec{f}}} \over {\partial{\vec{x}}} }\right|_{\vec{x}_{k-1},\vec{u}_{k-1}}


This requires taking the derivative of each state-equation with respect to each state.  Each row of
the Jacobian corresponds to a specific state-equation; each column of the matrix corresponds to a
specific system state.  Performing this operation results in:

.. math::

    F = I_{16} + { 
                   \begin{bmatrix} {
                                     \begin{array}{cccc}
                                                         {0_{3}} &
                                                         {I_{3}} &
                                                         {0_{3 \times 4}} &
                                                         {0_{3}} &
                                                         {0_{3}}
                                                         \cr
                                                         {0_{3}} &
                                                         {0_{3}} &
                                                         {\partial{v}\partial{q}} &
                                                         {0_{3}} &
                                                         {-{^{N}{R}^{B}}}
                                                         \cr
                                                         {0_{4 \times 3}} &
                                                         {0_{4 \times 3}} &
                                                         {{{1} \over {2}} \cdot \Omega} &
                                                         {-{{1} \over {2}} \cdot \Xi} &
                                                         {0_{4 \times 3}}
                                                         \cr
                                                         {0_{3}} &
                                                         {0_{3}} &
                                                         {0_{3 \times 4}} &
                                                         {0_{3}} &
                                                         {0_{3}}
                                                         \cr
                                                         {0_{3}} &
                                                         {0_{3}} &
                                                         {0_{3 \times 4}} &
                                                         {0_{3}} &
                                                         {0_{3}}
                                     \end{array}
                   } \end{bmatrix}
                 } \cdot {dt}


The one new term in the matrix, :math:`{\partial{v}\partial{q}}` is derived in the Appendix B and
listed below.

.. math::

    {\partial{v}\partial{q}} \equiv {
                                      2 \cdot \overline{Q}_{F} \cdot { \begin{bmatrix} {
                                                                                         \begin{array}{cc} 
                                                                                                             {0} &
                                                                                                             {\begin{pmatrix} { {\vec{a}^{B}} } \end{pmatrix} ^{T}}
                                                                                                             \cr
                                                                                                             {\vec{a}^{B}} &
                                                                                                             {-\begin{bmatrix} { {\vec{a}^{B}} \times } \end{bmatrix}}
                                                                                         \end{array}
                                                                       } \end{bmatrix}
                                                                     }
                                    }


where :math:`\overline{Q}_{F}` is:

.. math::

    \overline{Q}_{F} &= {
                          \begin{bmatrix} {
                                            \begin{array}{cccc} 
                                                                {q_{1}} &
                                                                {q_{0}} &
                                                                {-q_{3}} &
                                                                {q_{2}}
                                                                \cr
                                                                {q_{2}} &
                                                                {q_{3}} &
                                                                {q_{0}} &
                                                                {-q_{1}}
                                                                \cr
                                                                {q_{3}} &
                                                                {-q_{2}} &
                                                                {q_{1}} &
                                                                {q_{0}}
                                            \end{array}
                          } \end{bmatrix}
                        } \\
                        {\hspace{5mm}} \\
                     &= {
                          \begin{bmatrix} {
                                            {\vec{q}_{v}} \hspace{5mm} {q_0 \cdot I_{3} + \begin{bmatrix} { {\vec{q}_{v}} \times } \end{bmatrix}}
                          } \end{bmatrix}
                        }


and

.. math::

    \vec{a}^{B} = \vec{a}_{meas}^{B} - \vec{a}_{bias}^{B}
