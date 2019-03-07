OpenIMU300RI Evaluation Kit Overview
====================================

.. contents:: Contents
    :local:

.. toctree::
    :maxdepth: 3


OpenIMU300RI Evaluation Kit Introduction
----------------------------------------

The OpenIMU evaluation kit is a hardware platform used to evaluate the 
OpenIMU300RI inertial navigation system and develop various applications 
based on this platform.  It is supported by the Aceinna Navigation Studio, 
which provides easy access to the features of the  
OpenIMU300RI and explains how to integrate the device in a custom design.
The Components section below provides the contents of the kit.

.. note::

    An external DC power supply is required.  The power supply must provide 400mA at 4.9VDC to 32VDC.
 

+-------------------------------------------------+------------------------------------------------+
| .. figure:: ../media/OpenIMU300RI_DevKit.png    | .. figure:: ../media/OpenIMU300RI-EvalKit.png  |
|    :height: 300                                 |    :height: 500                                |
+-------------------------------------------------+------------------------------------------------+
||   **OpenIMU300RI Evaluation Kit Fixture with** || **OpenIMU300RI Evaluation Kit**               |
||   **OpenIMU300RI unit and JTAG Header Board**  |                                                |
+-------------------------------------------------+------------------------------------------------+

OpenIMU300RI Evaluation Kit components
----------------------------------------
    
    **OpenIMU300RI unit**
            
        OpenIMU300RI is 9 DOF (degrees of freedom) fully calibrated inertial unit. It is used as the base for development custom
        inertial navigation applications.  


    **OpenIMU300RI Evaluation Kit fixture and JTAG header board**

        The OpenIMU300RI unit and the JTAG header board are connected to a text fixture.
        The OpenIMU300RI Evaluation JTAG header board provides the means to connect the kit to an ST Link debugger.
        The JTAG header board is connected to the unit with a calbe that goes through a drilled hole in the back of the unit.

            
    **OpenIMU300RI Breakout Cable**

        A breakout cable is included that provides a connector to the OPENIUM300RI unit and to two 9-pin D-sub connectors; one for RS23s communication and the other for CAN Bus communication.
    
    **ST-Link debugger**

        The ST-Link debugger is a standard JTAG SWD debugger provided by STMicroelectronics company. 
        It used for in-system debugging of applications via SWD interface.

        
    **SWD (JTAG) connector**

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
        

    **OpenIMU evaluation kit power**

        Set the external DC power supply set to 12V.
        Power the kit up by turning on the power supply.


    **Communication with IMU from PC**

        The unit communicates to the PC through the UART RS232 9-pin D-sub connector, which would be connected to a PC via
        the PC's RS232  9-pin D-Sub or via a RS232-to-USB adapter.
        





OpenIMU Evaluation Kit Important Notice
-------------------------------------------

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
  

