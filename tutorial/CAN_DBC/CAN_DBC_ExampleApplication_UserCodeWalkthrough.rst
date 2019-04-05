CAN DBC Example Application User Code Walkthrough
***************************************************

.. contents:: Contents
    :local:

This page provides a walkthrough of the DBC example application code directories and files.

.. note::

    *   The *Data Acquisition Task* task source code in the DBC Example Application is almost identical to the *Data Acquisition Task* task
        source code in the J1939 Example Application.  There are minor cosmetic differences that don't affect functionality ... otherwise
        they are the same.
    *   The "CAN Communication Task" for the DBC application is much simpler than that for the J1939 application, as there are
        fewer DBC messages than J1939 messages.
    *   The source files in the 'src/user' directory of the DBC Example Application are all almost identical with those in the
        DBC Example Application, with the notable exception of the following:
        *   *UserConfiguration.c" function.  The User Configuration for the DBC application is simpler than that in the J1939 application.
        *   There are no CAN related user messaging source files, as they have not been developed yet.
        *   The DBC example application *UserMessaging.c* is much simpler than the J1939 Example Application's *UserMessagingUart.c*

*'include' directory*

    *FreeRTOSConfig.h*.  Provides application level '#define' literal constants.  Customer modification of any of these literals is not advised.

    *osresources.h*.  Currently provides application semaphore declarations.

    *taskDataAcquisition.h*.  Provides function prototype APIs for the Data Acquisition task, which are called in user application code.

*'include/API' directory*

    *userAPI.h*.  Provides a few '#define' literal constants and API function prototypes for functions called from user application code

*'lib/DBC/include'* directory

    *can.h*.   Provides typedefs, '#define' literal constants and API function prototypes used by functions in the *'lib/DBC/src'* directory.

    *dbcfile.h*.  Provides typedefs, '#define' literal constants, and DBC API function prototypes.

    *taskCanCommunicationDBC.h*.   Currently provides the API function prototype for the *TaskCANCommunicationDBC* function, which
    provides the code for the CAN Communication task.

*'lib/DBC/src'* directory

    *taskCANcommunicationDBC.c*. This file contains a single function, *TaskCANCommunicationDBC*, which implements the DBC example application CAN Communication Task.  The function description is as follows:

    *   *Task Initialization*.  The first two calls are to the same first two calls as in the J1939 application:
        *InitCANBoardConfiguration_GPIO* and *InitCommunication_UserCAN*, which initialize the CAN interface.  The third call is to *dbc_initialize*, which is described below in the *dbc_file.c* information below.

    *   *Task Event Loop*.  The event loop functionality is as follows:

        *   Wait for semaphore to be released
        *   Waiting for the right loop count*.  Once every *dbcPacketRateDivider* loops through the event loop, the *dbc_process*
            function is called, which is described below in the *dbc_file.c* information below.

    *can.c*.  This source files provide low-level CAN functions that are used within the CAN Communication Task.  Their usage is explained above.

    *dbc_file.c*.  This file contains the functions that provide the DBC example application personality:

    *   *checksum*.  Calculates a 1 byte checksum.
    *   *send_dbc_packet*.  Calls *CAN_Transmit* to transmit the message passed into the *send_dbc_packet* function
        as an input parameter.
    *   *dbc_initialize*.  Initializes the global transmit buffer linked list and calls *_CAN_Configure* to configure the CAN interface.
    *   *find_dbc_desc*. Finds the next available transmit buffer on the linked list.
    *   *dbc_send_rate*.  Prepares a DBC "Rate Data" message.

        *   Calls *find_dbc_desc* to get a transmit buffer.
        *   Calls *GetRateData_degPerSec*, which is a function in the Platform Library which translates rate data from
            radians to degrees.
        *   Inline code sets up the transmit buffer with scaled rate data.  This function does not actually transmit the buffer.

    *   *dbc_send_accel*.  Prepares a DBC "Acceleration Data" message.

        *   Calls *find_dbc_desc* to get a transmit buffer.
        *   Calls *GetAccelData_mPerSecSq*, which is a function in the Platform Library which translates acceleration
            data from g's to meters/(second**2).
        *   Inline code sets up the transmit buffer with scaled acceleration data.  This function does not actually transmit the buffer.

    *   *dbc_send_angle*.  Prepares a DBC "Euler Angle Data" message.

        *   Calls *find_dbc_desc* to get a transmit buffer.
        *   Calls *EKF_GetAttitude_EA*, which is a function in the Platform Library which gets the EKF algorithm Euler Angles
        *   Inline code sets up the transmit buffer with scaled Euler Angle data.  This function does not actually transmit the buffer.

    *   *dbc_transmit*.  Gets transmit buffers off the linked list that are active and calls *send_dbc_packet*, which is described above.

    *   *dbc_process*.  Performs the following:

        *   Using the input bitfield parameter "packetsToTransmit", checks the bitmask for each packet type
            (*Euler Angle Data*, "Rate Data", and "Acceleration Data")
        *   If the bitmask for a packet type matches, the appropriate data packet is sent using *dbc_send_angle* and/or
            *dbc_send_rate* and/or *dbc_send_accel*.
            Calls *dbc_transmit*, which is called above.

    *   *dbc_transmit_isr*  Calls *dbc_transmit*

*'src'* directory

    *main.c*.  The 'C' main function performs application initialization, creates applicable FreeRTOS tasks, creates
    applicable FreeRTOS semaphores, then passes control to the FreeRTOS scheduler, which does not return.

    *taskDataAcquisition.c*.  The contents of this file are identical with the same file in the J1939 application, with the
    exception that the DBC application file of two lines that set and clear a GPIO.

*'src/user'* directory

    *dataProcessingAndPresentation.c*.  This file is identical to the same file in the J1939 application, except for the name of one include file.  The J1939 application includes *UserMessagingCAN.h* while the DBC application includes *UserMessaging.h*

    *UserAlgorithm.c*.  The contents of this file are identical with the same file in the J1939 application, with the
    exception that the J1939 application uses one additional include file, *sae_j1939.h"
    *UserConfiguration.c* and *UserConfiguration.h*.

    *   There are three sets of functions in this file.  The first set initializes the User Configuration data structure
        with valid default values and, if necessary, saves the User Configuration data structure into EEPROM.
        The second set of functions handle Set and Get commands, which are addressed later.  The third set of functions provides
        boolean returns for user configuration parameter settings.

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
        *   *ApplySystemParameters* - Conditionally called in *userInitConfigureUnit* to apply the Rate Cutoff Frequency,
            the Acceleration Cutoff Frequency, and the Orientation in the User Configuration into the ECU Configuration.
            It also calls *activateUARTMessaging*, which is in the *UserMessagingUart.c* file.  That function sets the UART baud rate and packet output rate.

    *   Second set - Includes the following Get and Set command related functions:

        *   *is_valid_config_command* - Determines if the current Set command is valid, based on checking the PF parameter for
            "Global" (i.e. broadcast) value of '255' decimal.  Then it determines if the PS parameter is one of the
            PS values for this unit's valid PS values.
        *   Get command response functions: *GetEcuAddress* and *GetEcuBaudRate* return the associated parameter
            value from the User Configuration data.
        *   Set command response functions:  *SetEcuBaudRate*, *SetEcuPacketType*, *SetEcuPacketRate*, *SetEcuFilterFreq*,
            and *SetEcuOrientation* - set the corresponding User Configuration parameter to the input parameter value.

    *UserMessaging.c*.  This file does not contain any code.

    *UserMessaging.h*.  This file contains typedefs, '#define' literal constants and API function prototypes for DBC messaging.

*FreeRTOS library*.  The source files and header files for the FreeRTOS library are provided.  The locations for the directories for these files has not been finalized, but the base folder will be named "*FreeRTOS library*".  There will be 'src' and 'include' subdirectories.  The content of those files will not be addressed here.

*OpenIMU-misc-library*.  The "miscellaneous" library contains the example algorithm functions, example math functions, UART function, and support functions.

*OpenIMU300-platform-library*.  The source code for the Platform Library will not be provided to users. The header files that contain the API function prototypes for the Platform Library will be included when those files are packed into user visible directories.

*STI32F405 MCU Library*.  This library provides the low-level STI32F405 SoC functions.  The content of those files will not be addressed here.  Users should not modify any of these files.
