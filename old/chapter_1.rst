
Introduction
************

Manual Overview
---------------

This manual provides a comprehensive introduction to ACEINNA’s DMU281ZA,
DMU381ZA, and DMU481ZA Series Inertial System products (**D**\ ynamic
**M**\ easurement **U**\ nit 281ZA, 381ZA, or 481ZA). As the
functionality of the different series are identical (only the
performance specs are different), for simplicity all references will be
to the DMUx81ZA, where X is 2, 3, or 4. For users wishing to get started
quickly, please refer to the two-page quick start guide included with
each evaluation kit shipment. `Table 1 <\l>`__ highlights the content in
each section and suggests how to use this manual.

                  **Table 1 Manual Content**

+-----------------------------------+-------------------------------------+
| **Manual Section**                | **Who Should Read?**                |
+-----------------------------------+-------------------------------------+
| **Section** `1 <\l>`__:           | All customers should read           |
|                                   | sections `1.1 <\l>`__ and           |
| Manual Overview                   | `1.2 <\l>`__.                       |
+-----------------------------------+-------------------------------------+
| **Section** `2 <\l>`__:           || Customers designing the electrical |
|                                   | and mechanical interface to the     |
| Interface                         || DMUx81ZA series                    |
|                                   | products should read Section        |
|                                   | `2 <\l>`__.                         |
+-----------------------------------+-------------------------------------+
| **Section** `3 <\l>`__:           | All customers should read Section   |
|                                   | `3 <\l>`__.                         |
| Theory of Operation               |                                     |
|                                   || As the DMUx81ZA Series products    |
|                                   | are inter-related, use the chart    |
|                                   || at the beginning of Section        |
|                                   | `3 <\l>`__ to ensure that you get   |
|                                   || an overview of all of the          |
|                                   | functions and features of your      |
|                                   || DMUx81ZA Series system. For        |
|                                   | example, if you have purchased an   |
|                                   || INSx81ZA, you should read not      |
|                                   | only the section on the INSx81ZA,   |
|                                   || but also familiarize yourself      |
|                                   | with the theory of operation for    |
|                                   || the IMUx81ZA, VGx81ZA, and         |
|                                   | AHRSx81ZA. The INSx81ZA builds on   |
|                                   || the capabilities of the IMUx81ZA,  |
|                                   | VGx81ZA and AHRSx81ZA.              |
+-----------------------------------+-------------------------------------+
| **Section** `4 <\l>`__:           || Customers who want product         |
|                                   | configuration tips for operating    |
| Application Guide                 || the DMUx81ZA Series Inertial       |
|                                   | Systems in a wide range of          |
|                                   || applications – fixed wing, rotary  |
|                                   | wing, unmanned vehicles, land       |
|                                   || vehicles, marine vessels, and      |
|                                   | more, should review the part of     |
|                                   || Section `4 <\l>`__ that is         |
|                                   | relevant to your application.       |
|                                   || Note: INS and AHRS DMUx81ZA        |
|                                   | Series units are preconfigured      |
|                                   || for airborne applications with     |
|                                   | “normal” dynamics. VGx81ZA Series   |
|                                   || units are preconfigured for land   |
|                                   | applications with “automotive       |
|                                   || testing” dynamics. All DMUx81ZA    |
|                                   | Series products allow for           |
|                                   || complete flexibility in            |
|                                   | configuration by the user.          |
+-----------------------------------+-------------------------------------+
| **Section** `5 <\l>`__:           || Customers designing the software   |
|                                   | interface to the DMUx81ZA series    |
| SPI Port Interface                || products SPI Port should review    |
|                                   | Section `5 <\l>`__.                 |
+-----------------------------------+-------------------------------------+
| **Section**                       || Customers designing the software   |
| `6 <\l>`__-`9 <\l>`__:            | interface to the DMUx81ZA series    |
|                                   || products UART Port should review   |
| UART Port Interface               | Sections `6 <\l>`__-`9 <\l>`__.     |
+-----------------------------------+-------------------------------------+

.. _overview-of-the-dmux81za-series-inertial-systems:

Overview of the DMUx81ZA Series Inertial Systems
------------------------------------------------

This manual provides a comprehensive introduction to the use of
ACEINNA’s DMUx81ZA Series Inertial System products listed in `Table
2 <\l>`__. This manual is intended to be used as a detailed technical
reference and operating guide. ACEINNA’s DMUx81ZA Series products
combine the latest in high-performance commercial MEMS
(Micro-electromechanical Systems) sensors and digital signal processing
techniques to provide a small, cost-effective alternative to existing
IMU systems.

            **Table 2 DMUx81ZA Series Feature Description**

+-------------------------------+---------------------------------------+
| **Product**                   | **Features**                          |
+-------------------------------+---------------------------------------+
| IMUx81ZA (-200,-209,-409)     || 6-DOF Digital IMU, 9-DOF Digital     |
|                               | IMU Standard Range,9-DOF Digital      |
|                               || IMU High Range                       |
+-------------------------------+---------------------------------------+
| VGx81ZA (-200,-400)           | 6-DOF IMU plus Roll and Pitch         |
|                               | Standard Range, High Range            |
+-------------------------------+---------------------------------------+
| AHRSx81ZA (-200, -400)        || 9-DOF IMU (3-Axis Internal           |
|                               | Magnetometer) plus Roll, Pitch,       |
|                               || and Heading Standard Range, High     |
|                               | Range                                 |
+-------------------------------+---------------------------------------+
| INSx81ZA (-200, -400)         || 9-DOF IMU (3-Axis Internal           |
|                               | Magnetometer) with interface for      |
|                               || External GPS Receiver plus           |
|                               | Position, Velocity, Roll, Pitch,      |
|                               || and Heading Standard Range, High     |
|                               | Range                                 |
+-------------------------------+---------------------------------------+

The DMUx81ZA Series is ACEINNA’s fourth generation of MEMS-based
Inertial Systems, building on over a decade of field experience, and
encompassing thousands of deployed units and millions of operational
hours in a wide range of land, marine, airborne, and instrumentation
applications. It is designed for OEM applications, and is a
pin-compatible upgrade to the popular DMUx81ZA series.

At the core of the DMUx81ZA Series is a rugged 6-DOF (Degrees of
Freedom) MEMS inertial sensor cluster that is common across all members
of the DMUx81ZA Series. The 6-DOF MEMS inertial sensor cluster includes
three axes of MEMS angular rate sensing and three axes of MEMS linear
acceleration sensing. These sensors are based on rugged, field proven
silicon bulk micromachining technology. Each sensor within the cluster
is individually factory calibrated for temperature and non-linearity
effects during ACEINNA’s manufacturing and test process using automated
thermal chambers and rate tables.

Coupled to the 6-DOF MEMS inertial sensor cluster is a high performance
microprocessor that utilizes the inertial sensor measurements to
accurately compute navigation information including attitude, heading,
and linear velocity through dynamic maneuvers (actual measurements are a
function of the DMUx81ZA Series product as shown in `Table 2 <\l>`__).
In addition, the processor makes use of internal magnetic sensor and
external GPS data to aid the performance of the inertial algorithms and
help correct long term drift and estimate errors from the inertial
sensors and computations. The navigation algorithm utilizes a
multi-state configurable Extended Kalman Filter (EKF) to correct for
drift errors and estimate sensor bias values.

Another unique feature of the DMUx81ZA Series is the extensive field
configurability of the units. This field configurability allows the
DMUx81ZA Series of Inertial Systems to satisfy a wide range of
applications and performance requirements with a single mass produced
hardware platform. The basic configurability includes parameters such as
baud rate (UART), clock speed (SPI), packet type, and update rate, and
the advanced configurability includes the defining of custom axes and
how the sensor feedback is utilized in the Kalman filter during the
navigation process.

The DMUx81ZA Series is packaged in a light-weight, rugged, unsealed
metal enclosure that is designed for cost-sensitive commercial and OEM
applications. The DMUx81 can be configured to output data over a SPI
Port or a low level UART serial port. The port choice is user controlled
by grounding the appropriate pin on the connector. The DMUx81 low level
UART output data port is supported by ACEINNA’s NAV-VIEW 3.X, a powerful
PC-based operating tool that provides complete field configuration,
diagnostics, charting of sensor performance, and data logging with
playback.

