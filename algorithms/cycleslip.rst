Cycle Slip Detection
====================

One of the various methods for detecting and identifying cycle slips is to obtain the triple-difference (TD)
observations of carrier phases first. By triple differencing the observations (that is, at two adjacent data collection
epochs differencing double-difference (DD) observations which is differencing between receivers followed by
differencing between satellites) biases such as the clock offsets of the receivers and GPS satellites, and
ambiguities can be removed. The TD observables (in distance per second units) are

.. math::

 &\delta \nabla \Delta \Phi_1 = \delta \nabla \Delta \rho+\lambda_1\cdot \nabla \Delta C_1+\delta \nabla \Delta \tau
 +\delta \nabla \Delta s -\delta \nabla \Delta I+\delta \nabla \Delta b_1+\delta \nabla \Delta \epsilon_1\\
 &\delta \nabla \Delta \Phi_2 = \delta \nabla \Delta \rho+\lambda_2\cdot \nabla \Delta C_2+\delta \nabla \Delta \tau
 +\delta \nabla \Delta s-\gamma \cdot \delta \nabla \Delta I+\delta \nabla \Delta b_2+\delta \nabla \Delta \epsilon_2

where :math:`\Phi` is the measured carrier phase; :math:`\rho` is the geometric range from receiver to GPS satellite; 
:math:`\lambda` is the carrier wavelength ; :math:`C` is a potential cycle slip (in cycle units); :math:`\tau` is the 
delay due to the troposphere; :math:`s` is the satellite orbit bias; :math:`I` is the delay of L1 carrier phase due to
the ionosphere; :math:`\gamma={(\lambda_2/\lambda_1)}^2\approx 1.65`; :math:`b` is multipath; :math:`\epsilon` is
receiver system noise; subscripts “1” and “2” represent L1 and L2 carrier phases, respectively; and :math:`\nabla \Delta` 
and :math:`\delta \nabla \Delta` are the DD and TD operators, respectively.

In most GPS applications, regardless of surveying modes (static and kinematic) and baseline lengths (short, medium
and long), the effects of the triple-differenced biases and noise (i.e., atmospheric delay, satellite orbit bias,
multipath, and receiver system noise) are more or less below a few centimetres as long as observation sampling
interval is relatively short (e.g., sampling interval less than 1 minute). There could be exceptional situations such
as an ionospheric disturbance, extremely long baselines, and huge (rapid) variation of the heights of surveying
points in which the combined effects of the biases and noise can exceed the wavelengths of L1 and L2 carrier
phases. However, to simplify our discussion, we will assume, for the time being, that such situations can be
easily controlled through adjusting the sampling rate so that the combined effects of the biases and noise can be
reduced below a few centimetres. In the section of “Cycle-slip Candidates”, we will see that we can remove
this assumption.

**Cycle-slip Observables**

As revealed in the formula above, the geometric range should be removed to estimate the size of cycle slips. If we can
replace the TD geometric ranges with their estimates, then the TD carrier-phase prediction residuals become

.. math::

 &\delta \Phi_{TD1} = \delta \nabla \Delta \Phi_1-\delta \nabla \Delta \hat \rho=\lambda_1\cdot \nabla \Delta C_1+\epsilon'_1\\
 &\delta \Phi_{TD2} = \delta \nabla \Delta \Phi_2-\delta \nabla \Delta \hat \rho=\lambda_2\cdot \nabla \Delta C_2+\epsilon'_2

where

.. math::

 &\epsilon'_1 = \delta \rho_{TD}+\delta \nabla \Delta \tau+\delta \nabla \Delta s-\delta \nabla \Delta I+\delta \nabla \Delta b_1+\delta \nabla \Delta \epsilon_1\\
 &\epsilon'_2 = \delta \rho_{TD}+\delta \nabla \Delta \tau+\delta \nabla \Delta s-\gamma \cdot \delta \nabla \Delta I+\delta \nabla \Delta b_2+\delta \nabla \Delta \epsilon_2

and :math:`\delta \rho_{TD}(=\delta \nabla \Delta \rho-\delta \nabla \Delta \hat \rho)` represents the prediction
residuals of TD geometric ranges. As seen the above two formulas, therefore, the TD carrier-phase prediction residuals
will be a good measure for detecting and correcting cycle
slips if the effects of the residuals in the formula above are small.

**TD Geometric Range Estimation**

To obtain the estimates of the TD geometric ranges, we need an other observable which is immune from cycle
slips. The Doppler frequency and the TD pseudoranges can be used for this purpose. The former is preferable to
reduce the effects of the residuals in the formula above. Using the Doppler frequency at two adjacent data collection epochs,
we have

.. math::

 \delta \nabla \Delta \hat \rho_k = -(\nabla \Delta D_k+\nabla \Delta D_{k-1})/2

where :math:`D` is the Doppler frequency (in distance per second units); subscripts “k” and “k-1” represent the time tags of
two adjacent data collection epochs and the sign is reversed due to the definition of Doppler shift. For some
receivers for which the Doppler frequency is not available to users, the TD pseudoranges (somewhat nosier than the
Doppler frequency) can be used instead. Then the estimates of the TD geometric ranges are given as:

.. math::

 \delta \nabla \Delta \hat \rho_k=(\delta \nabla \Delta P_k-\delta \nabla \Delta P_{k-1})/\delta t

where :math:`P` is the measured pseudorange and :math:`\delta t(=t_k-t_{k-1})`
is the time interval between two adjacent data collection epochs.

**Cycle-slip Candidates**

Consider the first two moments of the TD carrier-phase prediction residuals

.. math::

 &E[\delta \Phi_{TDi}] = \lambda_i\cdot \nabla \Delta C_i, i=1,2\\
 &Cov[\delta \Phi_{TDi}]=Q_{TDi}

where :math:`E[\cdot]` and :math:`Cov[\cdot]` are the mathematical expectation and variance-covariance operators. Since there is no
redundancy to carry out statistical testing for the formula above, we will use it to obtain the cycle-slip candidates. In this case,
we need a priori information for the second moment. This can be obtained either through system tuning or adaptive
estimation. This means that we do not have to assume specific models for the biases and noise.

**Filtering of Cycle-slip Candidates**

When dual-frequency carrier phases are available, we can reduce, to a large extent, the number of cycle-slip
candidates using the TD geometry-free phase (a scaled version of which is called the ionospheric delay rate). The
TD geometry-free phase (in distance per second units) is 

.. math::

 \delta \Phi_{GF}&=\delta \nabla \Delta \Phi_1-\delta \nabla \Delta \Phi_2\\
 &=(\lambda_1 \cdot \nabla \Delta C_1-\lambda_2 \cdot \nabla \Delta C_2)+\epsilon"

where

.. math::

 \epsilon"=(\gamma -1)\delta \nabla \Delta I+(\delta \nabla \Delta b_1-\delta \nabla \Delta b_2)+(\delta \nabla \Delta \epsilon_1-\delta \nabla \Delta \epsilon_2)
 
We can also consider the first two moments of the TD geometry-free phase

.. math::

 &E[\delta \Phi_{GF}]=\lambda_1 \cdot \nabla \Delta C_1-\lambda_2 \cdot \nabla \Delta C_2\\
 &Cov[\delta \Phi_{GF}]=Q_{\delta GF}

An exceptional case is the combination-insensitive cycle-slip pairings of which the expectation in the above formula is close to zero.

**Cycle-slip Validation**

Fixing cycle slips in the TD observations is conceptually the same problem as resolving ambiguities in the DD
observations. Consider the linearized model of the TD observables:

.. math::

 &\pmb y = \pmb{Ac}+\pmb{Bx}+\pmb{e}, \pmb{c} \in Z^n, \pmb{x} \in R^u\\
 &Cov[\pmb{y}]=\pmb{Q}，

where **y** is the :math:`n \times 1` vector of the difference between the TD observations and their computed values; :math:`n` is the
number of measurements; **c** is the :math:`n \times 1` vector of the cycle-slip candidates; **x** is the :math:`u \times 1` vector of all other
unknown parameters including position and other parameters of interest; :math:`u` is the number of all other
unknowns except cycle slips; **A** and **B** are the design matrices of the cycle-slip candidates and the other
unknown parameters; **e** is the :math:`n \times 1` vector of the random errors.

The first step for cycle-slip validation is to search for the best and second best cycle-slip candidates which
minimize the quadratic form of the residuals. The residuals of least-squares estimation for cycle-slip candidates are given as:

.. math::

 \pmb{\hat v} = \pmb{y'}-\pmb{B\hat x},

where

.. math::

 &\pmb{y'} = \pmb{y} - \pmb{Ac}\\
 &\pmb{\hat x} = {(\pmb{B^TQ^{-1}B})}^{-1}\pmb{B^TQ^{-1}y'}

Then, discrimination power between two candidates is measured by comparing their likelihood. We follow a conventional discrimination 
test procedure similar to that described by Wang et al. [1998]. A test statistic for cycleslip validation is given by

.. math::

 d=\Omega_{e1}-\Omega_{e2}

where :math:`\Omega_{e1}` and :math:`\Omega_{e1}` are the quadratic form of the residuals of the best and second best candidates. 
A statistical test is performed using the following null and alternative hypotheses:

.. math::

 H_0:E[d] = 0, H_1:E[d]\ne0

A test statistic for testing the above hypotheses is given by

.. math::

 W = \frac{d}{\sqrt{Cov(d)}}

where

.. math::

 Cov[d] = 4{(\pmb{c_1-c_2})}^T\pmb{Q_c^{-1}}(\pmb{c_1-c_2})
 \pmb{Q_c^{-1}} = \pmb{Q}^{-1}-\pmb{Q^{-1}B{(B^TQ^{-1}B)}^{-1}}B^TQ^{-1}

If **y** is assumed as having a normal distribution, :math:`d` is normally distributed. Therefore, :math:`W` has mean 0 and
standard deviation 1 under the null hypothesis. Adopting a confidence level :math:`\alpha`, it will be declared that the
likelihood of the best cycle-slip candidate is significantly larger than that of the second best one if

.. math::

 W > N(0,1:1-\alpha)

Finally, a reliability test is carried out after fixing cycle slips in order to diagnose whether errors still remain in the observations. 
