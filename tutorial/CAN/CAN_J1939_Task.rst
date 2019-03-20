****************************
J1939 CAN Communication Task
****************************

.. contents:: Contents
    :local:


The *J1939 CAN Communication Task* (function *TaskCANCommunicationJ1939*) is unique to the J1939
application.  That task provides the following capabilities:

    *CAN Communication Setup*.   The following sub-tasks are performed to set up CAN bus communication:

        *   The GPIOs used for CAN communication must be initialized for electrical characteristics
            and initial values.
        *   The CAN bus baud rate must be set in accordance with the unit configuration.
        *   The unit's "name" must be initialized.  The "namme" consists of the 
            CAN function (J1939), manufacture's code (Acienna's code), and the identity 
            (the unit's serial number).
        *   Finally, the unit must be initialized by setting the CAN address and baud rate.
    
    *J1939 CAN Communication Task State Machine*.  The J1939 CAN Communication Task enters 
    an event loop, which performs processing of the state machine.  Some of the states are 
    hidden in low-level functions and won't be addressed here.  The high-level state machine, as 
    handled in the *TaskCANCommunicationJ1939* function operates as follows:

    *   *ECU_BAUDRATE_DETECT state*.  The high-level state machine starts in this state if "*baud rate detect*" 
        is enabled.  While in this state attempts are made to detect the baud rate.  When the baud rate is detected, 
        the state machine transitions to the *Address Claiming State*.  Once the state machines leaves 
        this state it is not re-entered.

    *   *Address Claiming State*.  If "*baud rate detect*" is not enabled, the state machine starts in this state.  
        This state is represented by the sub-states *ECU_CHECK_ADDRESS* and *ECU_WAIT_ADDRESS*.  Processing 
        will remain in the "Address Claiming" state until the state changes to the *ECU_READY* state or 
        until a loop counter exceeds the maximum value of *ADDRESS_CLAIM_RETRY*.  Processing that is asynchronous 
        to the task can cause the state to transition to the "*ECU_READY*" state. If the state doesn't change 
        asynchronously and a loop counter reaches the maximum value (ADDRESS_CLAIM_RETRY), then the state machine 
        is transitioned to the "ECU_READY* state. 

    *   *ECU_READY state*.  This is the steady state for the state machine.  The incming messages are  recieved and processed 
        and outgoing messages are built and transmitted in this state.

    *ECU_READY_STATE Processing*

        *Process incoming messages*.  While there is a valid input message to handle, the message type is determined 
        and the following is performed based on message type:

        *   Address Claim Type.  Address Claim messages are handled by the function "*process_address_claim*" in the 
            source file "*sae_j1939.c*".  Refer to that function for details.
        *   Request Type.  Request nessages are handled by 
            the function "*ProcessRequest*" in the source file "*UserMessaging.c*".  There are a number of request 
            message sub-types, which are handled as follows:

            *   Software Version Request or ECU ID Request.  The requested message is sent.

            *   Global Request.  The Global Request message has several request sub-types: Hardware BIT. Software BIT,
                Status Request, Packet Rate Request, Packet Type Request, Filter Settings Request, and Orientation Settings 
                Request.  Handling of the BIT and Status Requests require a messsage payload structure of significant size is 
                allocated and filled.  The requested message is sent.

            *   Address Claim Request.  Addresses on the CAN bus are arbitrated. every unit on the bus must arbitrate for
                its address.  The reply for the Address Claim message is built and the message is sent.

        *   Set Type.  The message is validated.  If valid, the set command is processed in the function "*ProcessEcuCommands*", which is
            located in the "*UserMessaging.c*" source file.  The following set smessage sub-types are handled:

            *   Algorithm Restart.  The algorithm state is reset to its default start state so that the algorithm is set to restart.  
                The algorithm configuration is saved.
            *   Save Configuration.  The current algorithm state is saved.
            *   Update Packet Rate.  The ECU and User packet rate configuration are set to the selected value.
            *   Update Transmitted Packets Set.  The transmitted packets set is updated so the selected packets are 
                sent during packet transmission.
            *   Update Filter Cutoff Frequencies.  The filter cutoff frequenices are updated so the next input sensor data set 
                uses the new cutoff frequencies.
            *   Update Unit Orientation.  The unit configuration is changed so the translation algorithm will use the new orientation 
                for the next sensor data set.
            *   Update Algorithm Behavior.  Enable or disable use of the algorithm.
            *   Reassign Bank0 and Bank1 PDU-Specific Codes.  The selected PDU-Specific bank data strucutre is updated per the message 
                contents and the ECU Configuration is updated.

        *Enqueue and transmit periodic data packet messages*. Packet enqueing is handled by the 
        function *ACEINNA_SAE_J1939_PACKET_ANGULAR_RATE* in the source file *UserMessaging.c*.  
        There are 3 periodic packet messages that can be enqueued and 
        sent: 
        
        *   SS2 packets when magnetometer is in use (*ACEINNA_SAE_J1939_PACKET_SLOPE_SENSOR*).
        *   Acceleration Packets (*ACEINNA_SAE_J1939_PACKET_ACCELERATOR*)
        *   Angular Rate Packets (*ACEINNA_SAE_J1939_PACKET_ANGULAR_RATE*).  
        
        For each type to be enqueued and sent a data structure is filled, then the aggregate of the packets data structures is
        packed into a messages and sent.




    




