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

    **Framework Application**

    All the examples that follow are based on the Framework application that ships installed on the
    OpenIMU.  This framework serves as a stating point upon which user-applications can be built.
    Descriptions of all common components are provided in the Framework Application Link below.


    **IMU Application**

    An IMU refers to a device that returns calibrated, inertial-sensor data.  This application forms
    the backbone of all other example applications as each requires inertial measurements to
    generate other results.


    **Static-Leveler Application**

    The Static-Leveler application uses accelerometer readings to measure the local gravity-field
    and compute the two-axis attitude (roll and pitch angles) of a body relative to the local-level
    frame.  A Leveler application could be used to provide stabilization for cameras and other
    systems that require accelerometer stable angular state.


    **VG/AHRS Application**

    The VG/AHRS application uses rate-sensors, accelerometers, and (in the case of an AHRS)
    magnetometers to compute the attitude and heading of a body in space.  Rate-sensors are
    used to propagate the attitude forward in time at high data-rates (DR) while
    accelerometers and magnetometers act as references, correcting for rate-sensor biases and
    attitude errors.


    **INS Application**

    The Inertial Navigation System (INS) application supports all of the features and operating
    modes of the VG/AHRS APP.  In addition it includes the additional capability of interfacing
    with an external GPS receiver and associated software running on the processor for
    computation of navigation information as well as orientation information.

**Robust CAN Example Applications**

    There are two CAN example applications, one for each CAN message protocol specifications used:

    *   *J1939 Messaging Specification*.  The original CAN bus messaging scheme
        was standardized by the SAE as Document J1939.

    *   *DBC Messaging Specification*.  The Database Communication (DBC) CAN message format is a
        proprietary format developed and released by the company Vector Informatik GmbH.  It is an
        optional addition for applications that use J1939 protocols.

.. note::   CAN provisioned units can run most of the example applications described in the *EZ Embed* section,
            so description of those applications are not provided here.  The CAN example applications implement the
            IMU Application functionality.


.. toctree::
    :maxdepth: 1
    :hidden:

    tutorial/OpenIMU_Core
    tutorial/Framework_App
    tutorial/IMU_App
    tutorial/Leveler_App
    tutorial/VG_AHRS_App
    tutorial/INS_App
    tutorial/CAN/CAN_J1939_Application
    tutorial/CAN/CAN_DBC_Application
