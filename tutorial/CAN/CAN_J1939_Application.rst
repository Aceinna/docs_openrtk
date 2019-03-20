CAN J1939 Example Application
*******************************

.. contents:: Contents
    :local:


J1939 CAN Interface Basic Information
--------------------------------------

The following page describes important basic information regarding the J1939 CAN Interface and J1939 messages.

.. toctree::
    :maxdepth: 1
    
    ./CAN_J1939_CAN_InterfaceBasicInformation

J1939 CAN Messages
------------------------

The following are the J1939 message categories:

*Set Command Messages*.  Set commands are used for other Electronic Control Units (ECUs) to configure the OpenIMU300RI on the network. 
All of the commands are broadcast messages. The receivers will decode the payload and will ignore the 
message if it is meant for another ECU.

.. blank line
.. toctree::
    :maxdepth: 1
    
    ./CAN_J1939_SetCommandMessages
.. blank line

*Data Packet Messages*.  The data packets are broadcast with a frequency that can be set by other ECUs. 

.. blank line
.. toctree::
    :maxdepth: 1
    
    ./CAN_J1939_DataPacketMessages

.. blank line

*Get Command Messages*.  The Get Commands include the *Hardware BIT*, *Software BIT*, and *Status* Commands.
They are provided to allow other ECUs to request information from the OpenIMU300RI.  The user can provide 
other Get commands as needed.

.. blank line
.. toctree::
    :maxdepth: 1
    
    ./CAN_J1939_GetCommandMessages


The CAN J1939 Example Application
---------------------------------------------

The CAN J1939 Example Application provides the framework for custom applications 
for the OpenIMU300RI unit.  The example can be used as is or customized to suit
system requirements.  The Software Block Diagram below depicts the connections between the application 
inputs, software processing components and application outputs.  The following page provides details
for the CAN J1939 Example Application.


.. figure:: ../../media/OpenIMU-FullSoftwareBlockDiagram.png
    :align: center

    *Software Block Diagram*

.. toctree::
    :maxdepth: 1

    ./CAN_J1939_ApplicationDetails    





