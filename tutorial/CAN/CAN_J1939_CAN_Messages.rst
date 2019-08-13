CAN J1939 Example Application CAN Messages
******************************************

In next chapter provided description of the J1939 messages used in OpenIMU300RI application examples.
Users can keep the implemented messages as is, modify them, or add new messages.

The following message categories are used:

    **Requests**

        Some requests are broadcast.  In such cases, all other nodes on the network will decode the
        payload and ignore the request if it is meant for another ECU.

        *Set Requests*. Set requests are used by Electronic Control Units (ECUs) to configure the OpenIMU300RI on the network.

        *Get Requests*. Get requests are used for requesting information from the OpenIMU300RI.  If the request is for the OpenIMU300RI, it will build and send a response packet to the requesting node.

    **Data Packets**

        Data packets are broadcast periodic messages with controllable rates, usually from 0 Hz (quiet mode) to 100Hz. The types of transmitted by OpenIMU300RI messages can be controlled by *Set Requests*.
        Also data packets can be arbitrary requested from OpenIMU by external ECUs using *Get Requests*.


.. toctree::
    :maxdepth: 1
    :hidden:

    ./CAN_J1939_SetCommandMessages
    ./CAN_J1939_GetCommandMessages
    ./CAN_J1939_DataPacketMessages
