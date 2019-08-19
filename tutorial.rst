######################################################################
Tutorial - What The User Needs to Know to Build The First Application
######################################################################

.. contents:: Contents
    :local:

.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>

.. toctree::
    :maxdepth: 2


**OpenIMU Core**

    The OpenIMU Core is the foundation for the Platform application and
    all other example and custom applications.  However, it is not supplied as a separate application.
    The OpenIMU Core provides the Board Support Package (BSP),
    FreeRTOS, command line interface capability, filters, GPS interface capability, math functionality,
    and various utilities, including the base examples for the C-language main function and the data
    acquisition functionality.



**EZ Embed Example Applications**


    The following applications are implicitly based on use of an *EZ Embed** OpenIMU unit, such as the OpenIMU300ZA.

    To get you acquainted with the OpenIMU environment, let's walk through the development of the
    following applications:


    **IMU Application**

    The term Inertial Measurement Unit (IMU) refers to a device that returns calibrated inertial-sensor data.
    This application forms the backbone of all other example applications as each requires inertial measurements to
    generate other results.


    **Static-Leveler Application**

    The Static-Leveler application uses accelerometer readings to measure the local gravity-field
    and compute the two-axis attitude (roll and pitch angles) of a body relative to the local-level
    frame.  A Leveler application could be used to provide stabilization for cameras and other
    systems that require linear and rotational stability.


    **VG&AHRS Applications**

    The Vertical Gyro (VG) application and the Attitude and Heading Reference System (AHRS) application
    use rate-sensors, accelerometers, and (for the AHRS application)
    magnetometers to compute the attitude and heading of a body in space.  Rate-sensors are
    used to propagate the attitude forward in time at high output data-rates (ODR) while
    accelerometers and magnetometers act as references, correcting for rate-sensor biases and
    attitude errors.


    **INS Application**

    The Inertial Navigation System (INS) application supports all of the features and operating
    modes of the VG&AHRS applications.  In addition it includes the additional capability of interfacing
    with an external GPS receiver and associated software running on the processor for
    computation of navigation position information as well as orientation information.

**Robust CAN Example Applications**

|    The CAN example applications are implemented for OpenIMU300RI unit with CAN interface.
|    Next example applications available for OpenIMU300RI unit:

    *   IMU application which are using *SAE J1939 Messaging Standard*.
    *   VG_AHRS application which are using *SAE J1939 Messaging Standard*.
    *   INS application which are using *SAE J1939 Messaging Standard*.



.. toctree::
    :maxdepth: 1
    :hidden:

    tutorial/OpenIMU_Core
    tutorial/IMU_App
    tutorial/Leveler_App
    tutorial/VG_AHRS_App
    tutorial/INS_App
    tutorial/CAN/CAN_J1939_Application
