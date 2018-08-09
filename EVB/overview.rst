Overview
========

.. contents:: Contents
    :local:

1. Introduction
-------------

The OpenIMU evaluation kit is a hardware platform to evaluate the OpenIMU300ZA
inertial navigation system and develop various applications based on provided platform.
Supported by the Aceinna Navigation Studio the kit provides easy access to the features 
OpenIMU300ZA and explains how to integrate the device in a custom design.
The OpenIMU evaluation kit include OpenIMU300ZA, evaluation board with various interface
connectors and test adapter for mounting OpenIMU300ZA unit.
 
.. image:: ../media/EvalKit.png  
 
2. Components
------------


- OpenIMU Evaluation board, which includes:

	- Virtual COM-port USB interface, providing connectivity to OpenIMU300ZA unit from PC

	- Connector for programming and debugging target via Serial Wire Debug (SWD) interface

	- Connector for interfacing OpenIMU300ZA from custom-designed system.

	- Test terminals for connecting oscilloscope or logic analyzers to the dedicated OpenIMU300ZA signals.

- OpenIMU300ZA unit. Please note, that it installed on the bottom side of evaluation board. 

- Test fixture adapter for convenient aligned mounting of OpenIMU evaluation board and OpenIMU300ZA unit 
- ST-Link debugger for in-system development of application code 
     
2.1 OpenIMU300ZA unit
~~~~~~~~~~~~~~~~~~~~~~~

       
OpenIMU300ZA is 9 DOF (degrees of freedom) fully calibrated inertial unit. It is used as the base for development custom
inertial navigation applications.

2.2 OpenIMU Evaluation board
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


OpenIMU Evaluation board designed to provide convenient way for communicating with OpenIMU300ZA unit from PC, to 
expose serial and SPI interfaces to developer and to debug applications using ST-Link debugger vis SWD interface.
       
2.3 OpenIMU test adapter
~~~~~~~~~~~~~~~~~~~~~~~~~~


OpenIMU test adapter used to firmly secure OpenIMU300ZA unit and Open IMU evaluation board in precisely aligned position. 
       
2.4 ST-Link debugger
~~~~~~~~~~~~~~~~~~~~~~

St-Link debugger is standard debugger provided by STMicroelectronics company. It used for in-system debugging of applications via SWD interface.
  
  
3. Open IMU evaluation board Headers and Connectors
---------------------------------------------------
  
3.1 Connector for plugging in OpenIMU300ZA unit (J2).   
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

J2 is 20-pin connector and it used for plugging OpenIMU300ZA unit into Open IMU evaluation board.
   

The connector pin functions are described in table below.

+-----------------+-------------------------+-----------------------+
| **Pin**         |   Main Function         | Alternative Function  |
|                 |                         |                       |
+-----------------+-------------------------+-----------------------+
| 1               || Output. Inertial-Sensor|| Can be used as GPIO  |
|                 | Sampling Indicator      ||(IO3)                 |
|                 || (sampling upon         |                       |
|                 | falling edge)           |                       |
+-----------------+-------------------------+-----------------------+
| 2               || Synchronization Input  |                       |
|                 |  1PPS Input             |                       |
|                 || (External GPS)         |                       |
+-----------------+-------------------------+-----------------------+
| 3               || User UART TX  (Output) | SPI Clock (SCLK)      |
|                 |                         |     (Output)          |
+-----------------+-------------------------+-----------------------+
| 4               | User UART RX  (Input)   | SPI Data Output       |
|                 |                         | (MISO)                |
+-----------------+-------------------------+-----------------------+
| 5               | GPS UART TX (Output)    | SPI Data Input (MOSI))|
+-----------------+-------------------------+-----------------------+
| 6               | GPS UART RX  (Input)    | SPI Chip Select (SS)  |
+-----------------+-------------------------+-----------------------+
| 7               || Data Ready (SPI        || SPI/UART Interface   |
|                 || Communication Data)    || Selector             |
+-----------------+-------------------------+-----------------------+
| 8               |             External Reset (NRST))              |
+-----------------+-------------------------+-----------------------+
| 9               | GPIO Output             ||Can be used as GPI0   |
|                 |                         ||(IO2)                 |
+-----------------+-------------------------+-----------------------+
| 10              | Power VIN (3-5 VDC)     | Power VIN (3-5 VDC)   |
+-----------------+-------------------------+-----------------------+
| 11              | Power VIN (3-5 VDC)     | Power VIN (3-5 VDC)   |
+-----------------+-------------------------+-----------------------+
| 12              | Power VIN (3-5 VDC)     | Power VIN (3-5 VDC)   |
+-----------------+-------------------------+-----------------------+
| 13              | Power GND               | Power GND             |
+-----------------+-------------------------+-----------------------+
| 14              | Power GND               | Power GND             |
+-----------------+-------------------------+-----------------------+
| 15              | Power GND               | Power GND             |
+-----------------+-------------------------+-----------------------+
| 16              | SWDIO (SWD debug interface)                     |
+-----------------+-------------------------+-----------------------+
| 17              | External GPS UART TX    |Debug interface UART TX|
+-----------------+-------------------------+-----------------------+
| 18              | SWCLK (SWD debug interface)                     |
+-----------------+-------------------------+-----------------------+
| 19              | External GPS UART RX    |Debug Interface UART RX|
+-----------------+-------------------------+-----------------------+
| 20              | Reference voltage for SWD debug interface       |
+-----------------+-------------------------+-----------------------+

3.2 Extension Header (J4).   
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

OpenIMU evaluation board has 12-pin extension header. It designed to expose IMU interface signals to
external system. The extension header pin functions described in table below 


+-----------------+-------------------------+-----------------------+
| **Pin**         |   Main Function         | Alternative Function  |
|                 |                         |                       |
+-----------------+-------------------------+-----------------------+
| 1               | Power GND               | Power GND             |
+-----------------+-------------------------+-----------------------+
| 2               | Power GND               | Power GND             |
+-----------------+-------------------------+-----------------------+
| 3               | GPS UART RX  (Input)    | SPI Chip Select (SS)  |
+-----------------+-------------------------+-----------------------+
| 4               || Data Ready (SPI        || SPI/UART Interface   |
|                 || Communication Data)    || Selector             |
+-----------------+-------------------------+-----------------------+
| 5               || User UART TX  (Output) | SPI Clock (SCLK)      |
|                 |                         |     (Output)          |
+-----------------+-------------------------+-----------------------+
| 6               || Synchronization Input  |                       |
|                 |  1PPS Input             |                       |
|                 || (External GPS)         |                       |
+-----------------+-------------------------+-----------------------+
| 7               | GPS UART TX (Output)    | SPI Data Input (MOSI))|
+-----------------+-------------------------+-----------------------+
| 8               |             External Reset (NRST))              |
+-----------------+-------------------------+-----------------------+
| 9               | User UART RX  (Input)   | SPI Data Output       |
|                 |                         | (MISO)                |
+-----------------+-------------------------+-----------------------+
| 10              | GPIO Output (IO2)       | GPIO Input            |
|                 |                         |                       |
+-----------------+-------------------------+-----------------------+
| 11              | Power VIN  5 VDC        | Power VIN 5 VDC       |
+-----------------+-------------------------+-----------------------+
| 12              || Output. Inertial-Sensor||Can be used as GPIO   |
|                 | Sampling Indicator      ||(IO3)                 |
|                 || (sampling upon         |                       |
|                 | falling edge)           |                       |
+-----------------+-------------------------+-----------------------+

3.4 IMU interface type selector (P1).   
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Interface type selector used to select between IMU SPI and UART interface.
In UART mode pins 1-2, 3-4, 5-6 should be closed (jumpers should be in place).
In SPI mode pins 1-2, 3-4, 5-6 should be opened (jumpers should be removed).

3.5 PC to GPS UART connector (P2).   
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


If desired - IMU GPS UART can be routed to PC COM port (for example for modeling).
It can be done ONLY when IMU interface configured to be in UART mode (see 3.4) 
In this case pins 1-2 and 3-4 on this connector should be closed.
Otherwise remove jumpers not to interfere with possible external connections via J4.
 
3.6 SWD (JTAG) connector (P3).   
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


20-pin connector P3 used for connecting ST-Link or J-Link debuggers to the unit for
in-system debugging of applications via SWD interface. It has standard pin-out.

+-------------------+-------------------------+
| **Pin**           |   Main Function         |
|                   |                         |
+-------------------+-------------------------+
| 1                 | Vref                    |
+-------------------+-------------------------+
|2, 4, 6, 8, 10 , 12| GND                     |
|14, 16, 18, 20     |                         |
+-------------------+-------------------------+
| 7                 | SWDIO                   |
+-------------------+-------------------------+
| 9                 | SWCLK                   |
+-------------------+-------------------------+
| 15                | nRST                    |
+-------------------+-------------------------+
| 19                | 3.3V from debugger      |
+-------------------+-------------------------+
 
3.7 USB connector (j3).   
~~~~~~~~~~~~~~~~~~~~~~~~
 
USB connector used for powering up the IMU and evaluation board. Also its used to providing connectivity
from PC to IMU via virtual serial ports. Up to 3 exposed IMU serial interfaces can be routed to PC.  


4. OpenIMU evaluation board LED indicators
-------------------------------------------

Evaluation board has few LED indicators for visual monitoring of data traffic on serial ports:

**LED2** indicator reflects activity on RX line of IMU main (user) serial interface (traffic to IMU)

**LED1** indicator reflects activity on TX line of IMU main (user) serial interface (traffic from IMU)

**LED3** indicator while lit indicates presence of the power (in case switch SW1 is "ON")

**LED4** indicator reflects activity on GPIO3 (lit if high)
 
**LED5** indicator reflects activity on GPIO2 (lit if high)


5. Open IMU evaluation power
----------------------------

Power to OpenIMU evaluation board provided by USB.
To power system up - connect USB cable to connector J1 and turn "ON" switch SW1.
 

6. Communication with IMU from PC
----------------------------------------------


OpenIMU evaluation board has FTDI chip FT4232 installed. This chip provides 4 virtual serial ports.
When evaluation board set up to force IMU interface in UART mode (see p.3.4) up to 3 serial ports on IMU can be communicated to from PC.
When evaluation board connected to PC and power switch turned "ON" the board will appear among external devices as 4 consecutive serial ports.
First serial port is napped to IMU's main UART channel (pins 3 and 4 on J2), which is dedicated for sending periodic messages from IMU and sending commands
to IMU. Second serial port mapped to IMU's GPS UART channel (pins 5 and 6), which is dedicated to be used as GPS serial port and also can be used for modeling - sending
GPS data from PC.
Third serial port mapped to IMU's debug serial port, which can be used for sending diagnostics messages from IMU and/or as CLI interface to IMU.
   
   
7. OpenIMU Evaluation Kit Important Notice
----------------------------------

::

     This evaluation kit is intended for use for FURTHER ENGINEERING, DEVELOPMENT,
     DEMONSTRATION, OR EVALUATION PURPOSES ONLY. It is not a finished product and may not (yet) 
     comply with some or any technical or legal requirements that are applicable to finished products,
     including, without limitation, directives regarding electromagnetic compatibility, recycling (WEEE), 
     FCC, CE or UL (except as may be otherwise noted on the board/kit). Aceinna supplied this board/kit 
     "AS IS," without any warranties, with all faults, at the buyer's and further users' sole risk. The 
     user assumes all responsibility and liability for proper and safe handling of the goods. Further, 
     the user indemnifies Aceinna from all claims arising from the handling or use of the goods. Due to
     the open construction of the product, it is the user's responsibility to take any and all appropriate
     precautions with regard to electrostatic discharge and any other technical or legal concerns.
     EXCEPT TO THE EXTENT OF THE INDEMNITY SET FORTH ABOVE, NEITHER USER NOR ACEINNA
     SHALL BE LIABLE TO EACH OTHER FOR ANY INDIRECT, SPECIAL, INCIDENTAL, OR
     CONSEQUENTIAL DAMAGES.
     No license is granted under any patent right or other intellectual property right of Aceinna covering
     or relating to any machine, process, or combination in which such Aceinna products or services might 
     be or are used.
  

