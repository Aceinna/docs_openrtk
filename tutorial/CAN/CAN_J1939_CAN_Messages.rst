CAN J1939 Example Application CAN Messages
******************************************

Users can keep the implemented messages as is, modify them, or add new messages.

The CAN J1939 Example Application provides the following message categories:

    **Requests**

        Some requests are broadcast.  In such cases, all other nodes on the network will decode the
        payload and ignore the request if it is meant for another ECU.

        *Set Requests*.  Set requests are used for other Electronic Control Units (ECUs) to configure the OpenIMU300RI on the network.

        *Get Requests*. Get requests are provided to allow other ECUs to request information from the OpenIMU300RI.  If the request is for the OpenIMU300RI, it will build and send a response packet to the requesting node.

    **Data Packets**

        Data packets are broadcast with a frequency that is internally controlled, but can be set by other ECUs
        using *Set Requests*.


.. toctree::
    :maxdepth: 1
    :hidden:

    ./CAN_J1939_SetCommandMessages
    ./CAN_J1939_GetCommandMessages
    ./CAN_J1939_DataPacketMessages
