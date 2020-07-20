EVK Vehicle Installation
=================================

.. .. contents:: Contents
    :local:



Reference coordinate frames
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
In order to install the OpenRTK330 EVB on vehicle for driving test, a few reference frames listed below has to be defined  

 * **The IMU body frame** is defined as below in the figure, and by default the INS solution of OpenRTK330 is provided at the center of navigation of the IMU.

    .. figure:: ../media/imu_body_xyz.jpeg
        :width: 5.0 in
        :height: 5.0 in
   
 * **The vehicle frame** is defined as

   * x-axis: points out the front of the vehicle in the driving direction
   * z-axis: points down to the ground
   * y-axis: completes the right-handed system
 * **The local level navigation frame** is defined as

   * x-axis: points north 
   * z-axis: points down parallel with local gravity
   * y-axis: points east 
 * **The user output frame** is used to transfer the INS solution to a user designated position.

Depends on the vehicle installation of the OpenRTK330 system, user has to configure two types of offsets to make the GNSS integrated INS solution work
 
 * Translation offset
   
   * *GNSS antenna lever-arm*: GNSS position is estimated to the phase center of the GNSS antenna, and INS position is estimated to the center of the navigation of the IMU. The translation from the IMU center to the phase center of the GNSS antenna has to be known and applied to the integrated system via user configuration of the antenna lever-arm. The GNSS/INS integrated solution outputs position at the IMU center.
   * *User output lever-arm*: If user wants the above GNSS/INS integrated solution output at a more useful position, the translation between the IMU center and the designated point of interest has to be known and applied via user configuration of point of interest lever-arm.
 * Rotation offset: If the axes of the IMU body frame of the installed OpenRTK330 unit is not aligned with the vehicle frame, the orientation of the IMU relative to the vehicle also has to be known and applied via user configuration of rotation angles between the IMU body frame and vehicle frame. 


