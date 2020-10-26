
What is OpenRTK?
=================

.. contents:: Contents
    :local:
    

OpenRTK is an open source hardware and software platform for development of high-performance navigation and localization applications on top of multi-constellation, multi-frequency Global Navigation Satellite System (GNSS) chips, a family of low-drift pre-calibrated Inertial Measurement Units (IMU) and cloud based server supports.

    * **Hardware**

      * OpenRTK hardware features of a multi-frequency, multi-constellation GNSS chipset from STMicroelectronics (aka ST), a triple-redudant 6-axis IMU sensor module from Aceinna, and an embedded STM32 ARM Cortex-M4 MCU with floating-point computation support for complex positioning engine
      * Spare I/O and ports for external sensors such as Odometer, camera for enhanced sensor fusion navigation
      * There comes two form-factors as follows:

        +----------------+------------------------------------------------------------------------+
        | **Model**      |     **Description**                                                    |
        +----------------+------------------------------------------------------------------------+
        |  OpenRTK330LI  | Inertial Navigation System Module – Industrial Grade                   |
        +----------------+------------------------------------------------------------------------+
        |  RTK330LA      | Inertial Navigation System Module – Automotive Grade (Contact Aceinna) |
        +----------------+------------------------------------------------------------------------+


    * **Software**

      * OpenRTK embedded software (i.e. the module firmware) is developped on top of the standard STM32 Cortex MCU library
      * Utilizes the FreeRTOS as the real time operating system for MCU
      * Provides a cost-free embedded environment and toolchain using `VS Code <https://code.visualstudio.com/>`_ and the associated Aceinna extension (based on `PlatformIO <https://platformio.org/>`_) 
      * Features with open-sourced firmware in the drivers and user interfaces, user can use or modify the provided firmware code to utilize or customize:

        - raw IMU data generation in sensor data extraction, pre-filtering and output data rate/format/interface and so on
        - UART input/output baudrate/mode/messages
        - CAN input/output mode/messages
        - Ethernet driver and input/output mode/messages
        - SPI driver
        - Bluetooth driver

      * Features with proprietary positioning engine library (NOT open-sourced):

            * GNSS Real Time Kinematic (RTK) positioning engine
            * GNSS/IMU integrated Inertial Navigation System (INS) positioning engine
            

    * **Cloud Service**

      * OpenRTK cloud service provides Networked Transport of RTCM via Internet Protocol (NTRIP) server and caster service for GNSS correction data
      * Provides online `developer site <https://developers.aceinna.com/>`_ for user interface

        - Web GUI
        - Data and algorithm simulation
        - Database for storage
        - Live support forum
        