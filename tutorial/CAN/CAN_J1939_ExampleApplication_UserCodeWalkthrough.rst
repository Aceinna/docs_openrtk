CAN J1939 Example Application User Code Walkthrough
***************************************************

.. contents:: Contents
    :local:

This page provides a walkthrough of the J1939 example application code directories and files.

*'include' directory*

    *FreeRTOSConfig.h*.  Provides application level '#define' literal constants.  Customer modification of any of these literals is not advised.

    *osresources.h*.  Currently provides application semaphore declarations.

    *taskDataAcquisition.h*.  Provides function prototype APIs for the Data Acquisition task, which are called in user application code.

*'include/API' directory*

    *userAPI.h*.  Provides a few '#define' literal constants and API function prototypes for functions called from user application code

*'lib/J1939/include'* directory

    *can.h*.   Provides typedefs, '#define' literal constants and API function prototypes used by functions in the *'lib/J1939/src'* directory.

    *sae_j1939.h*.  Provides typedefs, '#define' literal constants and API function prototypes used by functions in the *'lib/J1939/src'* and
    'src/user' directories.

    *taskCanCommunicationJ1939.h*.   Currently provides the API function prototype for the *TaskCANCommunicationJ1939*
    function, which provides the code for the CAN Communication task.  The source file that provides the
    *TaskCANCommunicationJ1939* function is *'lib/J1939/src/taskCANcommunicationJ1939.c'*

*'lib/J1939/src'* directory

    *taskCANcommunicationJ1939.c*.  This source file contains a single function, *TaskCANCommunicationJ1939*, which implements the CAN Communication
    Task by calling Platform Library functions and user supplied functions:

    *   CAN Task Initialization:

        *   *InitCANBoardConfiguration_GPIO* - This is a Platform Library function.  The API function prototype is in
            *taskCanCommunicationJ1939.h*
            which is in the *'include' directory*.  This function initializes SoC GPIOs used for CAN communication.
        *   *InitCommunication_UserCAN* - This function is in 'can.c' in the same directory as this function.
            Along with other CAN network initialization, it initializes the CAN interface using the 'baudrate' code that is passed in.
        *   Inline code - Initializes the J1939 OpenIMU300RI Name, which includes the Acienna Manufacturer code.
            The value of the code is '823' decimal and the serial number of the unit, which is stored in
            non-volatile memory during manufacturing.
        *   *sae_j1939_initialize* - This function is in *sae_j1939.c* in the same directory as this function.
            This function provides general initialization of all task data structures.

    *   CAN Task Event Loop:

        *   Wait for the semaphore to be released by the task synchronization implementation.
        *   One time state functionality.  The "Baudrate Detect" state and "Address Claim" state functionality is only
            executed once.  The state is checked after that in the event loop, but once the state enters "Ready" state, it stays in that state.

            *   Enters "Baudrate Detect" state and stays in that state until the CAN network baudrate has been
                set and detected.
            *   Enters "Address Claim" state and stays there until an address has been claimed, at which time the
                state transitions to the Ready state.
                The state is checked after that in the event loop, but once the state enters "Ready" state, it
                stays in that state.
        *   Ready state functionality.

            *   *ecu_process*.  This function is in *sae_j1939_slave.c*  in the same directory as this
                function. It handles all J1939 Get and Set messages.
            *   *EnqeuePeriodicDataPackets" - This function is in *UserMessagingCAN.c* in the '*src/user*'
                directory.  It enqueues data packets for the data packet type or types selected by nodes on the CAN network.
            *   *ecu_transmit* - This functions is in *sae_j1939_slave.c*  in the same directory as this function. It
                simply adds the data packet or packets into the outgoing CAN message queue, which is serviced by low-level
                CAN functions.

    *can.c*, *sae_j1939.c*, and *sae_j1939_slave.c*.  These source files provide low-level CAN functions that are used within the CAN Communication Task.  Their usage is explained above.

*'src'* directory

    *main.c*.  The 'C' main function performs application initialization, creates applicable FreeRTOS tasks, creates applicable FreeRTOS
    semaphores, then passes control to the FreeRTOS scheduler, which does not return.

    *taskDataAcquisition.c*.  This source file contains a single function, *TaskDataAcquisition*.  This function implements
    the Data Acquisition Task by calling Platform Library functions and a user supplied function:

    *   Task Initialization:

        *   *TaskDataAcquisition_Init* - A Platform Library function. The API function prototype for this function is
            in *taskDataAcquisition.h*, which is addressed above.  This function sets up the internal timer used
            for synchronizing all tasks.
        *   *initUserDataProcessingEngine* - This is an example of a user defined function.  It initializes user algorithm
            parameters.  The header file that provides the API function prototype is *userAPI.h" which is in the '*include/API*' directory.

    *   In the event loop:

        *   *PrepareToNewDacqTickAndProcessUartMessages* - A Platform Library function. Sets up the next 200Hz
            Data Acquisition tick and processes UART user messages.  The API function prototype for this function
            is in the "*taskDataAcquisition.h*" in the *'CAN_J1939/include' directory*.
        *   *GetSensorsData* - A Platform Library function. This function acquires data from the internal sensors,
            applies a user selected low-pass filter, and applies calibration parameters for scaling, temperature
            compensation, bias removal, and unit misalignment generally caused by  caused by non-orthogonal orientation
            of sensors inside the unit relative to the unit frame.
            The API function prototype for this function is in the "*taskDataAcquisition.h*" in the
            *'CAN_J1939/include' directory*.
        *   *handleOverRange* - A Platform Library function.  Ensures that data from overly sensitive sensors is
            within the range of the sensors.The API function prototype for this function is in the "*bitAPI.h" file
            in the Platform Library
        *   *inertialAndPositionDataProcessing* - This is an example for a user supplied function, which is in
            the sourcefile *dataProcessingAndPresentation.c* described below.
        *   *platformHasMag* and *MagAlign* - Platform Library functions.  Called after *inertialAndPositionDataProcessing*.
            If the unit is equipped with a magnetometer sensor, collects data for Hard/Soft Iron calibration.

*'src/user'* directory

    *dataProcessingAndPresentation.c*.  Includes the following functions:

        *   *initUserDataProcessingEngine*.  This function is addressed above in the "Task Initialization" description.
        *   *inertialAndPositionDataProcessing*.  This function gets sensor data, conditionally outputs debug data of
            the sensor data, then calls *RunUserNavAlgorithm*, which implements the user algorithm.

    *UserAlgorithm.c* and *UserAlgorithm.h*  Includes the following functions:

    *   *InitUserAlgorithm* - Addressed above.
    *   *RunUserNavAlgorithm* - Sets up data structures, calls *_Algorithm*, provides the processed data in globally
        accessible data structures, and conditionally provides debug outputs of the processed data.
    *   *_Algorithm* - Implements the user algorithm.

        *   If the algorithm data structures have not been initialized, algorithm data structures are initialized.
        *   *EKF_Algorithm* - In this example, this function is called, which implements the EKF algorithm described in
            the "EKF Algorithms" section found in the contents pane to the left.

    *UserConfiguration.c* and *UserConfiguration.h*.  There are three sets of functions in this file.

    *   First set - Includes the following User Configuration functions:

        *   *userInitConfigureUnit* - Initializes the "User Configuration" data structure with valid default values
        *   *SaveUserConfig* - save the User Configuration data structure in EEPROM.
        *   *LoadDefaultUserConfig* - Loads the constant default user data structure into RAM and conditionally saves the default
            into EEPROM.
        *   *SaveEcuConfig* - Copies the parameters in the ECU Configuration data structure that have corresponding User
            Configuration parameters into the User Configuration and conditionally calls *SaveUserConfig* to save the user configuration into EEPROM
        *   *SaveEcuAddress* - sets the User Configuration address parameter to the value passed in and calls "SaveUserConfig*
            to save the User Configuration into EEPROM
        *   *ApplyEcuSettings* - Called in *userInitConfigureUnit* to set corresponding parameter values in the
            ECU Configuration data structure to the input User Configuration data structure.
        *   *ApplySystemParameters* - Conditionally called in *userInitConfigureUnit* to apply the user selected Rate Cutoff Frequency,
            the Acceleration Cutoff Frequency, and the Orientation in the User Configuration into the ECU Configuration.
            It also calls *activateUARTMessaging*, which is in the *UserMessagingUart.c* file.  That function sets the UART baud rate and packet output rate and activates user-defined UART messaging functionality.

    *   Second set - Includes the following Get and Set command related functions:

        *   *is_valid_config_command* - Determines if the current Set command is valid, based on checking the PF parameter for
            "Global" (i.e. broadcast) value of '255' decimal.  Then it determines if the PS parameter is one of the
            PS values for this unit's valid PS values.
        *   Get command functions: *GetEcuAddress* and *GetEcuBaudRate* handle the Get commands and return the associated parameter
            value from the User Configuration data.
        *   Set command functions:  *SetEcuBaudRate*, *SetEcuPacketType*, *SetEcuPacketRate*, *SetEcuFilterFreq*,
            and *SetEcuOrientation* handle Set commands.  They set the corresponding User Configuration parameter to the
            input parameter value or values.

    *   Third set - Includes the following functions that return true/false values for User parameters:

        *   *CanTermResistorEnabled*. Returns the true/false value of the *CAN Terminator Resistor Enabled* User parameter.
        *   *CanBaudRateDetectionEnabled*.  Returns the true/false value of the *CAN Baudrate Detect Enabled* User parameter.
        *   *UseAlgorithm*.    Returns the true/false value of the *Run Algorithm* bit of the *UserBehavior* User parameter.

    *UserMessagingCAN.c* and *UserMessagingCAN.h*.  This file contains three functions:

    *   *ProcessRequest* - This function handles CAN Get Requests and the Address Claim request. It
        validates the request. For valid Get requests, it builds an output data structures and calls a function to
        send the output packet.  For the Address Claim request it calls *process_request_pg*, which is in the *sae_j1939.c" file.
        That function calls *send_address_claim*, which is also in the  *sae_j1939.c" file.  That function builds and sends an address claim message.
    *   *EnqeuePeriodicDataPackets*.  This function builds an output data structures and sends the data packet for
        selected data packets types.
    *   *ProcessEcuCommands*.  This function handles CAN Set commands:

        *   *Reset Algorithm*.  The user algorithm data structures are initialized back to their default initial conditions
            and a response message is sent.
        *   *Save Configuration*.  The ECU Configuration is saved to EEPROM and  a response message is sent.
        *   *Packet Rate*.  The output data rate is set to the input parameter value in the User and ECU configuration
            and a response message is sent.
        *   *Packet Type*.  The selected packet type(s) are set in the User and ECU Configuration and a response message is sent.
        *   *Filter Cutoff Frequencies*.  The selected cutoff frequency values for acceleration and rate sensors are set in
            the User and ECU Configuration and a response message is sent.
        *   *Orientation*.  The specified orientation values for 6DOF sensors are set in the User and ECU Configuration
            and a response message is sent.
        *   *User Behavior*.  The user supplied "restart on over range" and "dynamic motion" parameter values are set in
            the User and ECU Configuration and a response message is sent.
        *   *Set Bank0 PS Parameter Values*.  The Bank0 PS set includes the *Algorithm Reset* PS and
            BIT and status PS values.  The values in the User and ECU Configuration are set and a response message is sent.
        *   *Set Bank1 PS Parameter Values*.  The Bank1 PS set includes the those for *Packet Rate*, *Packet Type*,
            *Filter Cutoff Frequencies*, *User Behavior*, *Angle Alarm*, *Cone Alarm*, and *Acceleration*.
            The values in the User and ECU Configuration are set and a response message is sent.

    *UserMessagingUart.c* and *UserMessagingUart.h*.  This file contains the following functions:

    *   *checkUserPacketType*.  Determines whether the output  packet type is valid or not.  If it is valid, it sets the
        global *InputPacketType* or *OutputPacketType* value, depending on whether it is an input our output message.
    *   *userPacketTypeToBytes*.  Packs the *InputPacketType* or *OutputPacketType* value into a byte array.
    *   *setUserPacketType*.  If the input parameter packet type is valid sets *InputPacketType* or *OutputPacketType*
        and sets  the *User Payload Length* to the appropriate output length.  Finally, it calls the Platform Library function
        "platformSetOutputPacketCode", which sets the packet code in a Platform Library internal global
        configuration data structure.
    *   *getUserPayloadLength*.  Returns the *User Payload Length*
    *   *HandleUserInputPacket*.  Handles the following user inputs from the UART:

        *   *Reset*.  Calls the Platform Library *Reset* function, which merely sets a global reset flag to true.
        *   *Ping*. Builds an output data structure that includes the unit model number, build revision number, and
            unit serial number.
        *   *Get Version*.  Builds an output data structure that includes the software version number of the running software.

    *   *HandleUserOutputPacket*.  Outputs a UART message based on the value of the global *OutputPacketType* variable:

        *   *Test Packet*. Sets up output of a test packet that outputs a test packet count.
            This is only useful during development.
        *   *Output Data Packet*.  Sets up output of the acceleration, angular rate, and magnetometer sensor data.

    *   *WriteResultsIntoOutputStream*.  This function is currently empty, so the user is free to output
        data as needed to the UART.  The *Test Packet* and *Output Data Packet* packet data is available.

*FreeRTOS library*.  The source files and header files for the FreeRTOS library are provided and are also freely available from the FreeRTOS site.  The locations for the directories for these files has not been finalized, but the base folder will be named "*FreeRTOS library*".  There will be 'src' and 'include' subdirectories.  The content of those files will not be addressed here.

*OpenIMU-misc-library*.  The "miscellaneous" library contains the example algorithm functions, example math functions, UART function, and support functions.  The files and functions in this library are accessible to the user the user can update or add to theses files as needed.

*OpenIMU300-platform-library*.  The header files that contain the API function prototypes for the Platform Library will be included when those files are packed into user visible directories.  Users should not modify these files.  The content of those files will not be addressed here.

*STI32F405 MCU Library*.  This library provides the low-level STI32F405 MCU functions.  The content of those files will not be addressed here.  Users should not modify these files.
