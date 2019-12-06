
What is OpenRTK?
================

.. contents:: Contents
    :local:
    

*   OpenRTK is an open software platform for development of high-performance navigation and localization applications on top 
    of multi-constellation, multi-frequency Global Navigation Satellite System (GNSS) chips, a family of low-drift pre-calibrated 
    Inertial Measurement Units (IMU) and cloud based server supports .
*   OpenRTK hardware consists of a multi-frequency GNSS chipset a 3-axis rate sensor (gyro), 3-axis accelerometer platform, and 3-axis magnetometer module.
*   The module contains a low-power embedded ARM Cortex-M4 CPU with floating-point math support. RTK/PPP and GNSS/IMU integration 
    algorithms are embedded in firmware for up to centimetre accuracy in various situations.
*   Extra IO and Ports make connection of external peripherals such as Odometer, and other more advanced sensors possible.
*   OpenRTK server provides GNSS network and cloud computation services, which ensure users to achieve fast centimeter-decimeter level position services.
*   The OpenRTK hardware comes in different form-factors including:

**Hardware Configurations**

+-------+--------------+---------------------------------------------------------------+
| Type  | Part Number  | Hardware Features                                             |
+-------+--------------+---------------------------------------------------------------+
| *LC*  | OpenRTK300LC | Customer IMU Module without low-drift pre-calibration         |
+-------+--------------+---------------------------------------------------------------+
| *LI*  | OpenRTK300LI | Industrial IMU Module with low-drift pre-calibration          |
+-------+--------------+---------------------------------------------------------------+


**Open-Source Embedded Software**

*   OpenRTK hardware runs an open-source stack written on top of standard ARM Cortex libraries.
*   OpenRTK300 use FreeRTOS while OpenRTK330 uses a simple real-time scheduler.
*   The open-source stack includes EKF (Extended Kalman Filter) algorithms that can be used 
    directly or customized for application specific use.
*   The overall system loop is typically configured to run at 800Hz ensuring high quality 
    aliasing-free measurements for processing.
*   Also included in the OpenRTK embedded software platform are drivers for various GPS receivers, customizable SPI, 
    CAN, and UART messaging, and customizable settings that can be adjusted run-time and/or permanently.
*   A number of predefined settings are defined for baud rate, output date rate, sensor filter settings, and XYZ axis transformations.
*   The Core OpenRTK embedded software consists of the following:

    * FreeRTOS
    * Extended Kalman Filter Algorithms
    * High-Speed Deterministic Sampling
    * Messaging
    * GNSS RTK/PPP engine
    * GNSS/IMU integration
    * Accurate Time Service
    * Sensor Filtering
    * Settings Module for Dynamic and Permanent Unit Configuration
