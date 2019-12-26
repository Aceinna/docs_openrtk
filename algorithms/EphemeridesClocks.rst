GNSS Satellite Ephemerides and Clocks
=====================================

OpenRTK supports broadcast ephemerides and clocks for GPS, GLONASS, Galileo and BeiDou.
The following equations show the ephemeris and clock models used in RTKLIB. 

Broadcast ephemerides and clocks for GPS and Galileo
----------------------------------------------------

Broadcast ephemeris and SV clock parameters for GPS and Galileo are given in navigation
messages as:

.. math::

  \pmb{P}_{eph}(t_{oe},t_{oc},IOD) = {(a,e,i_0,\Omega_0,\omega,M_0,\Delta{n},\dot{I},\dot{\Omega},C_{us},C_{uc},C_{rs},C_{rc},C_{is},C_{ic},af_0,af_1,af_2,T_{GD})}^T

By using these parameters, the satellite position (antenna phase center position) :math:`\pmb{r}^s(t)` in ECEF, the satellite clock bias 
:math:`dT^s(t)` and clock drift :math:`dT^s(t)` are computed as: 

.. math::

   &{t}_k = t-t_{oe}\\
   &M = M_0 + (\sqrt{\frac{\mu}{a^3}}+\Delta{n})t_k\\
   &M = E-esinE\\
   &a = \frac{\sqrt{1-e^2}sinE}{cosE-e}\\
   &\phi=arctan\alpha+\omega\\
   &\delta{u} = C_{us}sin2\phi+C_{uc}cos2\phi\\
   &\delta{r} = C_{rs}sin2\phi+C_{rc}cos2\phi\\
   &\delta{i} = C_{is}sin2\phi+C_{ic}cos2\phi\\
   &u = \phi+\delta{u}\\
   &r = a(1-ecosE)+\delta{r}\\
   &i = i_0+\delta{i}+\dot{I}t_k\\
   &\Omega = \Omega_0+(\dot{\Omega}-\omega_e)t_k-\omega_et_{oe}\\
   &\pmb{r}^s(t)=r\begin{pmatrix}
         {cos\ u\ cos\ \Omega-sin\ u\ cos\ i\ sin\ \Omega}\\ 
         {cos\ u\ sin\ \Omega+sin\ u\ cos\ i\ cos\ \Omega}\\
         {sin\ u\ sin\ i}
         \end{pmatrix}\\
   &t_c = t-t_{oc}
   dT^s(t) = af_0+af_1t_c+af_2tc^2-\frac{2\sqrt{\mu}}{c^2}esqrt{A}sinE-bT_{GD}
   d\dot{T}^s(t) = af_1+2af_2t_c

where:

 :math:`\mu`: earth gravitational constant (:math:`3.9860050\times10^{14}\ m^3/s^2` 
 for GPS, :math:`3.986004418\times10^{14}\ m^3/s^2` for Galileo)

 :math:`\omega_e`: earth angular velocity (:math:`7.2921151467\times10^{-5}\ rad/s`)

 :math:`b=f_1^2/f_1^2` for :math:`L_i` pseudorange

 :math:`T_{GD}`: group delay parameters for GPS, :math:`B_{GD}` for Galileo (s)

The Kepler equation can be solved the following iteration by Newtonʹs method.

 .. math::
    
    &E_0 = M\\
    &E_{i+1} = E_i-\frac{E_i-esin\ E_i-M}{1-ecos\ E_i}\\
    &E = \lim_{i \to \infty}E_i

The broadcast ephemerides and clock are applied in case that the processing option ʺSatellite
Ephemeris/Clockʺ to ʺBroadcastʺ as well as GLONASS and BeiDou.


Broadcast ephemerides and clocks for GLONASS
--------------------------------------------

Broadcast ephemeris and clock parameters for GLONASS are given in the navigation messages as:

.. math::

  \pmb{P}_{eph}(t_b)=(x,y,z,v_x,v_y,v_z,a_x,a_y,a_z,\tau_n,\gamma_n)

The differential eauations for the satellite position :math:`\pmb{r}^s(t)={(x,y,z)}^T` and 
velocity :math:`\pmb{v}^s(t)={(v_x,v_y,v_z)}^T` in ECEF (PZ90.02) can be formed as:

.. math::

  &\frac{dx}{dt}=v_x, \frac{dy}{dt}=v_y, \frac{dz}{dt}=v_z\\
  &\frac{dv_x}{dt}=-\frac{\mu}{r^3}x-\frac{3}{2}J_2\frac{\mu a_e^2}{r^5}x(1-\frac{5z^2}{r^2})+\omega_e^2x+2\omega_ev_y+a_x\\
  &\frac{dv_y}{dt}=-\frac{\mu}{r^3}y-\frac{3}{2}J_2\frac{\mu a_e^2}{r^5}y(1-\frac{5z^2}{r^2})+\omega_e^2y+2\omega_ev_x+a_y\\
  &\frac{dv_z}{dt}=-\frac{\mu}{r^3}z-\frac{3}{2}J_2\frac{\mu a_e^2}{r^5}z(3-\frac{5z^2}{r^2})+a_z

where:

  :math:`a_e`: earth semi-major axis (6378136.0 m)

  :math:`\mu`: earth gravitational constant (:math:`398600.44\times10^9 m^s/s^2`)

  :math:`\omega_e`: earth angular velocity (:math:`7.292115\times10^{-5} rad/s`)

  :math:`J_x`: second zonal harmonic of the geopotential (:math:`1082626.7\times10^{-9}`)

  :math:`r=\sqrt{x^2+y^2+z^2}`

Note that two errata of GLONASS ICD 5.1 has be corrected in the models above. 

The satellite position :math:`\pmb{r}^s(t)` and velocity :math:`\pmb{v}^s(t)` at the time :math:`t` can be derived by the RK4 
(Runge‐Kutta 4th order and stage) numerical integration to solve these differential equations 
with the initial satellite position :math:`\pmb{r}^s(t_b)` and velocity :math:`\pmb{v}^s(t_b)` at the reference 
time ::math:`t_b`. For satellite clock bias :math:`dT^s(t)` and drift :math:`dT^s(t)` at the epoch time :math:`t` are also derived as: 

.. math::

  dT^s(t) = -\tau_n+\gamma_n(t-t_b)
  dT^s(t) = \gamma_n

The relativistic effect in the satellite clock are included in the GLONASS clock parameters. So the 
relativistic correction is not applied in this case. 


Broadcast ephemerides and clocks for BeiDou
-------------------------------------------

For BeiDou satellites, the similar ephemeris and clock parameters as GPS, Galileo are provided in the 
navigation messages as: 

.. math::

  \pmb{P}_{eph}(t_{oe},t_{oc}) = {(a,e,i_0,\Omega_0,\omega,M_0,\Delta{n},\dot{I},\dot{\Omega},C_{us},C_{uc},C_{rs},C_{rc},C_{is},C_{ic},af_0,af_1,af_2,T_GD)}^T

For MEO and IGSO satellites of BeiDou, the same formulations for GPS ephemeris and clock, except for
:math:`\mu = 3.986004418\times10^14`, :math:`\omega_e = 7.2921150\times10^(-5)` rad/s and the time :math:`t` is expressed in BDT. 

To obtain the satellite position :math:`\pmb{r}^s(t)` of BeiDou GEO satellites at the time :math:`t` in BDT, the equation above should be replaced by: 

.. math::

  &\Omega = \Omega_0+\dot{\Omega}_{t_k}-\omega_et_{oe}\\
  &\pmb{r}^s(t)=r\pmb{R}_z(\omega_et_k)\pmb{R}_x(-5°)r\begin{pmatrix}
    {cos\ u\ cos\ \Omega-sin\ u\ cos\ i\ sin\ \Omega}\\
    {cos\ u\ sin\ \Omega+sin\ u\ cos\ i\ cos\ \Omega}\\
    {sin\ u\ sin\ i}
    \end{pmatrix}

where:

 .. math::

   \pmb{R}_x{\theta} = \begin{pmatrix}
     1&0&0\\ 
     0&cos\ \theta&sin\ \theta\\ 
     0&-sin\ \theta&cos\ \theta
     \end{pmatrix}, 
   \pmb{R}_z{\theta} = \begin{pmatrix}
     cos\ \theta&sin\ \theta&0\\ 
     -sin\ \theta&cos\ \theta&0\\ 
     0&0&1
     \end{pmatrix}


Precise ephemerides and clocks
------------------------------

The precise ephemerides for GPS, GLONASS, Galileo and BeiDou are usually provided as SP3‐c files containing 
satellite positions and velocities (optional) at every 15 min or 5 min epochs. To obtain the satellite 
position at the time :math:`t`, an appropriate interpolation is needed. OpenRTK uses the fixed degree 
(:math:`n=10`) polynomial interpolation by Newton‐Nevilleʹs algorithm as: 

.. math::

  &P_{j,j}(t) = x_j      &(i\leq j\leq i+n)\\
  &P_{j,k}(t) = \frac{(t_k-t)P_{j,k-1}(t)+(t-t_j)P_{j+1,k}(t)}{t_k-t_j}\ \ \ \ &(i\leq j<k\leq i+n)

where :math:`n` is the degree of the polynomial for the interpolation and :math:`x(t_i),x(t_{i+1}),x(t_{i+2}),...,x(t_{i+n})`
are the ephemeris values for each components at the epochs :math:`t_i,t_{i+1},t_{i+2},...,t_{i+n}`. For example, in the :math:`n=4`
case, the interpolated value :math:`x(t)` at the time :math:`t` can be derived as:

.. math::

  &P_{i,i}(t) = x(t_i)&\ &\ &\ &\ \\
  &\ &P_{i,i+1}(t) &\ &\ &\ &\ \\
  &p_{i+1, i+1}(t) = x(t_{i+1})&\ &P_{i,i+2}(t)&\ &\ &\ \\
  &\ &P_{i+1,i+2}(t)&\ &P_{i,i+3}(t)&\ \\ 
  &P_{i+2,i+2}(t)=x(t_{i+2})&\ &P_{i+1,i+3}(t)&\ &P_{i,4}(t)=x(t)\\
  &\ &P_{i+1,i+2}(t)&\ &P_{i+1,i+4}(t)&\ \\
  &P_{i+3,i+3}(t)=x(t_{i+3})&\ &P_{i+2,i+4}(t)&\ &\ \\
  &\ &P_{i+3,i+4}(t)&\ &\ &\ \\
  &P_{i+4,i+4}(t)=x(t_{i+4})&\ &\ &\ &\ \\

Note that precise ephemerides usually present the CoM (center of mass) positions of 
satellite not as the antenna phase center position. So users should correct the 
satellite antenna phase center offset to use the precise ephemerides.

In spite of the precise ephemeris high‐order polynomial interpolation, a simple linear 
interpolation is implemented for precise clocks provided as SP3‐c or clock RINEX files as: 

.. math::

  dT^s(t)=\frac{(t_{i+1}-t)dT^s(t_i)+(t-t_i)dT^s(t_{i+1})}{t_{i+1}-t_i}\ \ \ \ \ (t_i\leq t<t_{i+1})

For the precise clocks provided by IGS (International GNSS service), the relativistic effect should be
corrected as:

.. math::

  dT^s(t)=\frac{(t_{i+1}-t)dT^s(t_i)+(t-t_i)dT^s(t_{i+1})}{t_{i+1}-t_i}-2\frac{\pmb{r}^s{(t)}^T{\pmb{v}}^s(t)}{c^2}

where :math:`\pmb{r}^s(t)` and :math:`\pmb{v}^s(t)` are the satellite position and velocity derived from the precise ephemerides.

The precise ephemerides and clocks are applied in case that the processing option ʺSatellite
Ephemeris/Clockʺ to ʺPreciseʺ.


