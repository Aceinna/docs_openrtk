Overview
========

.. contents:: Contents
    :local:

.. toctree::
    :maxdepth: 3


Introduction
--------------

The OpenIMU evaluation kit is a hardware platform to evaluate the OpenIMU300RI
inertial navigation system and develop various applications based on provided platform.
Supported by the Aceinna Navigation Studio the kit provides easy access to the features 
OpenIMU300RI and explains how to integrate the device in a custom design.
The OpenIMU evaluation kit include OpenIMU300RI, evaluation board with various interface
connectors and test adapter for mounting OpenIMU300RI unit.

.. note::

    An external DC power supply is required.  The power supply must provide 250mA at 12V.
 

.. figure:: ../media/OpenIMU300RI-EvalKit.png
    :height: 600

    **OpenIMU300RI Evaluation Kit setup**




OpenIMU300RI Evaluation Kit components
----------------------------------------
    
    * OpenIMU300RI unit
            
        OpenIMU300RI is 9 DOF (degrees of freedom) fully calibrated inertial unit. It is used as the base for development custom
        inertial navigation applications.  The OpenIMU300RI unit provided with the Eval Kit provides a connection to a header for the STG-Link debugger.  This connection is not available for production units


    * OpenIMU Evaluation baseboard

        The OpenIMU300RI base board board provides a header for the ST Link debugger, which is connected to the
        unit through a drilled hole in the back.

            
    * ST-Link debugger

        The ST-Link debugger is a standard JTAG SWD debugger provided by STMicroelectronics company. 
        It used for in-system debugging of applications via SWD interface.

        
    * SWD (JTAG) connector

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
        

    * OpenIMU evaluation kit power

        * Set the external DC power supply set to 12V.
        * Power the kit up by turning on the power supply.



    * Communication with IMU from PC

        The unit communicates to the PC through the CAN bus and/or the UART.  
        The UART must be connected to an RS232 dongle.  The CAN bus signals must be connected to a CAN bus adapter.




OpenIMU Evaluation Kit Important Notice
---------------------------------------

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
  

