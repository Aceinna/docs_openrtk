Integer Ambiguity Resolution
============================

Once the estimated states obtained in the EKF measurement update, the float 
carrier‐phase ambiguities can be resolved into integer values in order to improve 
accuracy and convergence time. At first, the estimated states and their covariance 
matrix are transformed to DD (double-difference) forms by: 

.. math::

  &\hat{\pmb{x}}'_k=\pmb{G}\hat{\pmb{x}}_k(+)={(\hat{\pmb{r}}_r^T,\hat{\pmb{v}}_r^T,\hat{\pmb{N}}^T)}^T\\
  &\pmb{P}'_k=\pmb{GP}_k(+)\pmb{G}^T=\begin{pmatrix}
     \pmb{Q}_R&\pmb{Q}_NR\\
     \pmb{Q}_RN&\pmb{Q}_N\\
     \end{pmatrix}

where:


  :math:`\pmb{G}=\begin{pmatrix}\pmb{I}_{6\times6}&\ &\ &\ \\\ &\pmb{D}&\ &\ \\\ &\ &\pmb{D}&\ \\\ &\ &\ &\pmb{D}\end{pmatrix}`:
  SD (single-difference) to DD transformation matrix

In this transformation, the SD carrier‐phase biases are transferred to the DD carrier‐phase form in
order to eliminate receiver initial phase terms to obtain integer ambiguities :math:`\hat{\pmb{N}}` 
and their covariance :math:`\pmb{Q}_N`. In these formulas, the most appropriate integer vector 
:math:`\breve{\pmb{N}}` for the integer ambiguities is obtained by solving an ILS (integer least square) 
problem expressed as:

.. math::

  \breve{\pmb{N}}=\mathop {argmin}_{\pmb{N} \in \pmb{Z}}({(\pmb{N}-\hat{\pmb{N}})}^T\pmb{Q}_N^{-1}(\pmb{N}-\hat{\pmb{N}}))

To solve the ILS problem, a well‐known efficient search strategy LAMBDA and its extension
MLAMBDA are employed in OpenRTK. LAMBDA and MLAMBDA offer the combination of a linear
transformation to shrink the integer vector search space and a skillful tree‐search procedure in the
transformed space. The integer vector solution by these procedures is validated by the following
simple ʺRatio‐Testʺ. In the ʺRatio‐Testʺ, the ratio‐factor :math:`R`, defined as the ratio of the weighted sum of
the squared residuals by the second best solution :math:`\breve{\pmb{N}}_2` to one by the best 
:math:`\breve{\pmb{N}}`, is used to check the reliability of the solution. The validation threshold
:math:`R_{thres}` can be set by the processing option ʺMin Ratio to Fix Ambiguityʺ. Current OpenRTK just 
only supports a fixed threshold value.

.. math::
  
  R=\frac{{(\breve{\pmb{N}}_2-\hat{\pmb{N}})}^T{\pmb{Q}_N}^{-1}(\breve{\pmb{N}}_2-\hat{\pmb{N}})}{{(\breve{\pmb{N}}-\hat{\pmb{N}})}^T{\pmb{Q}_N}^{-1}(\breve{\pmb{N}}-\hat{\pmb{N}})}>R_{thres}

After the validation, the ʺFIXEDʺ solution of the rover antenna position and velocity :math:`\breve{\pmb{r}}_r`
and :math:`\breve{\pmb{v}}_r` are obtained by solving the following equation. If the validation failed, OpenRTK
outputs the ʺFLOATʺ solution  :math:`\hat{\pmb{r}}_r` and :math:`\hat{\pmb{v}}_r` instead.

.. math::

  \begin{pmatrix}\ \breve{\pmb{r}_r}\\\ \breve{\pmb{v}_r}\end{pmatrix}=&
  \begin{pmatrix}\ \hat{\pmb{r}_r}\\\ \hat{\pmb{v}_r}\end{pmatrix}-&
  \pmb{Q}_{RN}{\pmb{Q}_N}^{-1}(\hat{\pmb{N}}-\breve{\pmb{N}})

In case the processing option is set as the ʺFix and Holdʺ mode (Integer Ambiguity Resolution = Fix
and Hold) and the fixed solution properly validated by the previous test, the DD carrier‐phase bias
parameters are tightly constraint to the fixed integer values. For these purpose, RTKLIB inputs the
following ʺpseudoʺ measurements to EKF and updates EKF.

.. math::

  &\pmb{y}=\breve{\pmb{N}}\\
  &\pmb{h}(\pmb{x})=\pmb{Gx}\\
  &\pmb{H}(\pmb{x})=\pmb{G}\\
  &\pmb{R}=diag(\sigma_c^2,\sigma_c^2,\sigma_c^2,...)

where:

  :math:`\pmb{G}=\begin{pmatrix} \pmb{0}&\pmb{D} &\ &\ \\  \pmb{0}&\ &\pmb{D}&\ \\  \pmb{0}&\ &\ &\pmb{D} \end{pmatrix}`: 
  SD to DD transformation matrix

  :math:`\sigma_c`: constraint to fixed integer ambiguities (= 0.001 cycle).