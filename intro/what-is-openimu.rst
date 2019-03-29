
What is OpenIMU?
================

.. contents:: Contents
    :local:

*   OpenIMU is an open software platform for development of high-performance navigation and localization
    applications on top of a family of low-drift pre-calibrated Inertial Measurement Units (IMU).
*   OpenIMU hardware consists of a 3-axis rate sensor (gyro), 3-axis accelerometer platform, and 3-axis magnetometer module.
*   The module contains a low-power embedded ARM Cortex-M4 CPU with floating-point math support.  Extra IO and Ports make
    connection of external peripherals such as GPS, Odometer, and other more advanced sensors possible.
*   The OpenIMU hardware comes in different form-factors including:

**Hardware Configurations**

+-------+--------------+---------------------------------------------------------------+
| Type  | Part Number  | Hardware Features                                             |
+-------+--------------+---------------------------------------------------------------+
| *EZ*  | OpenIMU300ZA | Easy to Embed 3-5V UART/SPI Automotive Module                 |
+-------+--------------+---------------------------------------------------------------+
| *EZ*  | OpenIMU300ZI | (Future) Easy to Embed 3-5V UART/SPI Industrial Module        |
+-------+--------------+---------------------------------------------------------------+
| *CAN* | OpenIMU300RA | (Future) Rugged, Waterproof 5-32V CAN/RS232 Automotive Module |
+-------+--------------+---------------------------------------------------------------+
| *CAN* | OpenIMU300RI | Rugged, Waterproof 5-32V CAN/RS232 Industrial Module          |
+-------+--------------+---------------------------------------------------------------+
| *LC*  | OpenIMU300LA | Low-Cost Surface-Mount Component                              |
+-------+--------------+---------------------------------------------------------------+

**Open-Source Embedded Software**

*   OpenIMU hardware runs an open-source stack written on top of FreeRTOS and standard ARM Cortex libraries.
*   The open-source stack includes EKF (Extended Kalman Filter) algorithms that can be used directly or
    customized for application specific use.
*   The overall system loop is typically configured to run at 800Hz ensuring high quality aliasing-free
    measurements for processing.
*   Also included in the OpenIMU embedded software platform are drivers for various GPS receivers, customizable
    SPI, CAN, and UART messaging, and customizable settings that can be adjusted run-time and/or permanently.
*   A number of predefined settings are defined for baud rate, output date rate, sensor filter settings, and XYZ axis transformations.
*   The Core OpenIMU embedded software consists of the following:

    * FreeRTOS
    * Extended Kalman Filter Algorithms
    * High-Speed Deterministic Sampling
    * Messaging
    * GPS Drivers
    * Accurate Time Service
    * Sensor Filtering
    * Settings Module for Dynamic and Permanent Unit Configuration
