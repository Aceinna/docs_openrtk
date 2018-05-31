Application Guide
*****************

Introduction of Application
---------------------------

This section provides recommended advanced settings for tailoring the
DMUx81ZA Series of inertial systems to different types of application
and platform requirements.

Fixed Wing Aircraft
-------------------

A fixed-wing aircraft is a heavier-than-air craft where movement of the
wings in relation to the aircraft is not used to generate lift. The term
is used to distinguish from rotary-wing aircraft, where the movement of
the wing surfaces relative to the aircraft generates lift. The fixed
wing aircraft can range in size from the smallest experimental plane to
the largest commercial jet. The dynamic characteristics of the fixed
wing aircraft depends upon types of aircraft (i.g., glider, propeller
aircraft, and jet aircraft) and mission phases (i.e., launch, landing,
and maneuver). In order to meet application requirements, users must
dial in proper advanced settings so that the DMUx81ZA Series can provide
the best possible solution under given dynamic conditions. For example,
Table 14 provides the recommended advanced settings for four different
dynamic conditions.

     **Table 14 Recommended Advanced Settings for Fixed Wing Aircraft**

+-------------+-------------+-------------+-------------+-------------+
| **Recommen  | **AHRSx81Z  |             |             |             |
| ded         | A           |             |             |             |
| Product**   | or          |             |             |             |
|             | INSx81ZA**  |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| **Recommen  | **Dynamic   |             |             |             |
| ded         | Condition** |             |             |             |
| Settings**  |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
|             || Pre-launch | Launch      || Normal     | High        |
|             || known      |             || (Default)  |             |
|             || straight   |             |             |             |
|             || and level  |             |             |             |
|             || flight     |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| UseMags     | ON          | ON          | ON          | ON          |
+-------------+-------------+-------------+-------------+-------------+
| UseGPS      | ON          | ON (< 4g)   | ON          | ON (< 4g)   |
+-------------+-------------+-------------+-------------+-------------+
| FreelyInteg | OFF         | OFF*\*      | OFF         | OFF (< 2g)  |
| rate        |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| Stationary  | OFF         | OFF         | OFF         | OFF         |
| Yaw Lock    |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| Restart     | ON          | OFF         | OFF         | OFF         |
| Over Range  |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| Dynamic     | OFF         | ON          | ON          | ON          |
| Motion      |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| Turn Switch | 0.5 deg/s   | 0.5 deg/s   | 0.5 deg/s   | 0.5 deg/s   |
| Threshold   |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| XY Filter   | 5 Hz        | 5 Hz        | 5 Hz\*      | 15 Hz       |
| Accel       |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| Z Filter    | 5 Hz        | 5 Hz        | 5 Hz\*      | 15 Hz       |
| Accel       |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| Filter Rate | 20 Hz       | 20 Hz       | 20 Hz\*     | 20 Hz       |
| Sensor      |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+


    A cutoff frequency of filters may be varied depending on the
    fastest dynamic mode of the aircraft. For example, the conventional
    aircraft has five dynamic modes, short-period, phugoid, spiral,
    dutch-roll, and roll, and the fastest one is the roll mode. The
    natural frequency of this mode is around 6~8 radian/sec or (about 2
    Hz) in most cases. Therefore, the recommended filter setting would
    not reject desired frequency components (or dynamic modes) that one
    wants to capture. However, the larger the bandwidth (or cutoff
    frequency) is, the noisier the corresponding signal is, which may
    result in the performance degradation. If the aircraft is operated
    under severe vibrations, also, the recommended filter setting may
    need to be further reduced in order to reject the frequency
    components caused by the vibration.

    FreelyIntegrate should only be set to “ON” for severe launch
    conditions. Normal takeoff dynamics that a standard aircraft would
    experience will see the best performance with this setting in the
    “OFF” position.

Rotocraft
---------

Rotorcraft is a category of heavier-than-air flying machines that use
lift generated by rotors. They may also include the use of static
lifting surfaces, but the primary distinguishing feature being lift
provided by rotating lift structures. Rotorcraft includes helicopters,
autogyros, gyrodynes and tiltrotors.

The rotor blade dynamics itself is much faster than that of the fixed
wing aircraft and contains high frequency components. At the same time,
however, it may cause severe vibrations on the airframe. Also, the
overall dynamics (translational and rotational motion) of the rotor
craft is much slower than the fixed wing aircraft due to a mechanical
mechanism of rotors generating the aerodynamic forces and moments. Table
16 provides the recommended advanced settings for two different dynamic
conditions.

     **Table 15 Recommended Advanced Settings for Rotorcraft**

+-----------------------+-----------------------+-----------------------+
| **Recommended         | **AHRSx81ZA or        |                       |
| Product**             | INSx81ZA**            |                       |
+-----------------------+-----------------------+-----------------------+
| **Recommended         | **Dynamic             |                       |
| Settings**            | Condition**           |                       |
+-----------------------+-----------------------+-----------------------+
|                       | **Normal Dynamics**   | **High Dynamics**     |
|                       |                       |                       |
|                       |                       | **(with               |
|                       |                       | uncoordinated tail    |
|                       |                       | motion)**             |
+-----------------------+-----------------------+-----------------------+
| UseMags               | ON                    | ON                    |
+-----------------------+-----------------------+-----------------------+
| UseGPS                | ON                    | ON (< 4g)             |
+-----------------------+-----------------------+-----------------------+
| FreelyIntegrate       | OFF                   | OFF (< 2g)            |
+-----------------------+-----------------------+-----------------------+
| Stationary Yaw Lock   | OFF                   | OFF                   |
+-----------------------+-----------------------+-----------------------+
| Restart Over Range    | OFF                   | ON                    |
+-----------------------+-----------------------+-----------------------+
| Dynamic Motion        | ON                    | ON                    |
+-----------------------+-----------------------+-----------------------+
| Turn Switch Threshold | 1.0 deg/s*\*          | 30.0 deg/s*\*         |
+-----------------------+-----------------------+-----------------------+
| XY Filter Accel       | 5 Hz\*                | 5 Hz                  |
+-----------------------+-----------------------+-----------------------+
| Z Filter Accel        | 5 Hz\*                | 5 Hz                  |
+-----------------------+-----------------------+-----------------------+
| Filter Rate Sensor    | 20 Hz\*               | 20 Hz                 |
+-----------------------+-----------------------+-----------------------+

..

    The helicopter can change its heading angle rapidly unlike the
    aircraft which requires banking. A turn switch threshold that is too
    low may cause turn switch activation with high duty cycle causing
    random walk in roll and pitch angles due to low feedback gains.

    A cutoff frequency must be far away from major frequency
    components caused by the rotor vibration.


Land Vehicle
------------

Some examples of land vehicles are: Automobiles, trucks, heavy
equipment, trains, snowmobiles, and other tracked vehicles. Table 16
provides the recommended advanced settings for two different types of
application.

      **Table 16 Recommended Advanced Settings for Land Vehicle**

+-----------------------+-----------------------+-----------------------+
| **Recommended         | **VGx81ZA or          |                       |
| Product**             | INSx81ZA**            |                       |
+-----------------------+-----------------------+-----------------------+
| **Recommended         | **Dynamic Condition** |                       |
| Settings**            |                       |                       |
+-----------------------+-----------------------+-----------------------+
|                       | **Heavy Equipment     | **Automotive Testing  |
|                       | Application**         | (IMU/VG               |
|                       |                       | default)**            |
+-----------------------+-----------------------+-----------------------+
| UseMags               | ON\*                  | ON\*                  |
+-----------------------+-----------------------+-----------------------+
| UseGPS                | ON                    | ON (< 4g)             |
+-----------------------+-----------------------+-----------------------+
| FreelyIntegrate       | OFF                   | OFF                   |
+-----------------------+-----------------------+-----------------------+
| Stationary Yaw Lock   | OFF                   | OFF                   |
+-----------------------+-----------------------+-----------------------+
| Restart Over Range    | ON                    | OFF                   |
+-----------------------+-----------------------+-----------------------+
| Dynamic Motion        | ON                    | ON                    |
+-----------------------+-----------------------+-----------------------+
| Turn Switch Threshold | 5.0 deg/s             | 10.0 deg/s            |
+-----------------------+-----------------------+-----------------------+
| XY Filter Accel       | 5 Hz                  | 5 Hz                  |
+-----------------------+-----------------------+-----------------------+
| Z Filter Accel        | 5 Hz                  | 5 Hz                  |
+-----------------------+-----------------------+-----------------------+
| Filter Rate Sensor    | 20 Hz                 | 20 Hz                 |
+-----------------------+-----------------------+-----------------------+

..

    When not in distorted magnetic environment.

Water Vehicle
-------------

Water vehicle is a craft or vessel designed to float on or submerge and
provide transport over and under water. Table 17 provides the
recommended advanced settings for two different types of application.

     **Table 17 Recommended Advanced Settings for Water Vehicle**

+----------------------------+-------------------+-----------------+
| **Recommended Product**    | **INSx81ZA**      |                 |
+----------------------------+-------------------+-----------------+
| **Recommended Settings**   | **Application**   |                 |
+----------------------------+-------------------+-----------------+
|                            | **Surfaced**      | **Submerged**   |
+----------------------------+-------------------+-----------------+
| UseMags                    | ON\n              | ON\             |
+----------------------------+-------------------+-----------------+
| UseGPS                     | ON                | OFF             |
+----------------------------+-------------------+-----------------+
| FreeIntegrate              | OFF               | OFF             |
+----------------------------+-------------------+-----------------+
| Stationary Yaw Lock        | OFF               | OFF             |
+----------------------------+-------------------+-----------------+
| Restart Over Range         | OFF               | OFF             |
+----------------------------+-------------------+-----------------+
| Dynamic Motion             | ON                | ON              |
+----------------------------+-------------------+-----------------+
| Turn Switch Threshold      | 10 deg/s          | 5 deg/s         |
+----------------------------+-------------------+-----------------+
| XY Filter Accel            | 5 Hz              | 2 Hz            |
+----------------------------+-------------------+-----------------+
| Z Filter Accel             | 5 Hz              | 2 Hz            |
+----------------------------+-------------------+-----------------+
| Filter Rate Sensor         | 15 Hz             | 10 Hz           |
+----------------------------+-------------------+-----------------+

..

    When not in distorted magnetic environment.

☑ **EXAMPLE**

`Figure 8 <\l>`__ shows a typical flight profile of the fixed wing
aircraft and the corresponding advanced settings that one can configure
adaptively depending on a flight phase:

**Prelaunch** is the phase of flight in which an aircraft goes through a
series of checkups (hardware and software) on the ground before takeoff.
The aircraft is a static condition,

**Takeoff** is the phase of flight in which an aircraft goes through a
transition from moving along the ground (taxiing) to flying in the air,
usually along a runway. The aircraft is under horizontal acceleration
and may suffer from vibrations coming from an engine and ground contact
forces transmitted from its landing gear.

**Climb** is the phase of a flight, after take-off, consisting of
getting the aircraft to the desired flight level altitude. More
generally, the term ‘climb’ means increasing the altitude. The aircraft
is under vertical acceleration until it reaches the steady-state climb
rate.

**Straight and level flight** is the phase of flight in which an
aircraft reaches its nominal flight altitude and maintains its speed and
altitude. The aircraft is under equilibrium (See `Figure 8 <\l>`__).

**Maneuver** is the phase of flight in which an aircraft accelerates,
decelerates, and turns. The aircraft is under non-gravitational
acceleration and/or deceleration (See `Figure 8 <\l>`__).

**Descent** is the phase of flight in which an aircraft decreases
altitude for an approach to landing. The aircraft is under vertical
deceleration until it captures a glide slope (See `Figure 8 <\l>`__).

**Landing** is the last part of a flight, where the aircraft returns to
the ground (See `Figure 8 <\l>`__).

|flightprofile|

Figure 8 Typical flight profiles of fixed wing aircraft and the
corresponding advanced settings

.. |flightprofile| image:: media/image7.png
   :width: 5.5481in
   :height: 2.32891in
