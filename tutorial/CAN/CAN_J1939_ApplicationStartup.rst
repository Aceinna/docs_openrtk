J1939 Application Startup
-------------------------

*   **src/main.c**.  This is a typical "*main.c*" source file.  It includes a few simple utility functions and the *main* function.
    The main function and functions it calls provide the following functionality:

        *BoardInit* and *platformSetUnitCommunicationType* function calls.

            These functions apply board initialization values and set the CAN interface as the primary communication interface for the application.

            The low-level details aren't addressed here, but the source files are available for customer review.

        *platformInitConfigureUnit* call.  This function applies the configuration stored in EEPROM at the factory.  The configuration includes:

            The user UART channel is assigned.

            The unit's calibration and configuration data are read from EEPROM and stored in RAM data structures.  The configuration data set
            includes parameters that are not part of the user configuration data set.  The calibration and configuration data structures are
            defined in '.piolebdeps/OpenIMU300-platform-library/Core/include/xbow_configuration.h'.


        *userInitConfigureUnit* function call.  This function applies user-chosen configuration for the application.

            If the application hasn't been run before, the default user configuration stored as a constant data structure in the application
            into the user configuration data structure in RAM and stored into non-volatile Read/Write memory.  The User Configuration data
            set is described below.

            The user configuration in non-volatile memory is validated.

            If the user configuration data in non-volatile memory is valid, that data is read into and overrides the RAM
            user configuration data.  Otherwise, the default user configuration stored as a constant data structure in the application
            into the RAM data structure.

            The ECU Configuration data set includes all of the User Configuration data set parameters.  Those parameters from the ECU Configuration
            data set are copied from the User Configuration data set.

            If the EEPROM Configuration sector is locked, the currently configured filter frequency cutoff and orientation values are applied.
            Build date and time are stored in a local string pointer.

        *CreateTasks* function call:

            The following FreeRTOS tasks and semaphores are created:

            *   *TaskDataAcquisition* task
            *   *TaskCANCommunicationJ1939*.
            *   *Gyro Ready* (gyroReadySem) semaphore
            *   *Accelerometer Ready* (accelReadySem) semaphore
            *   *Magnetometer Ready* (magReadySem) semaphore
            *   *CAN Data* (canDataSem) semaphore
            *   *Data Acquisition* (dataAcqSem) semaphore

        *osKernelStart* call. The OS kernel takes over, which starts the OS scheduler, which starts the previously created tasks.

*   **User Configuration Data Set**

    *   *Default User Configuration Data Set Parameters*.  The default user configuration parameters for J1939 example application are used the
        first time the application is executed and when the user configuration parameters stored in EEPROM are not valid.
        The User Configuration data set parameters and their default values are provided in the following table:

        .. table:: *User Configuration Data Set Parameters and Default Values*
            :align: left

            +-----------------------+-----------+-----------------------+
            |  **Parameter**        | **Value** | **Meaning**           |
            +-----------------------+-----------+-----------------------+
            |  Baud Rate            | _ECU_250K | 250K baud             |
            +-----------------------+-----------+-----------------------+
            |  Packet Rate          | 1         | 100Hz                 |
            +-----------------------+-----------+-----------------------+
            || Accelerometer Filter | 25        | 25Hz                  |
            || Cutoff Frequency     |           |                       |
            +-----------------------+-----------+-----------------------+
            || Angular Rate Filter  | 25        | 25Hz                  |
            || Cutoff Frequency     |           |                       |
            +-----------------------+-----------+-----------------------+
            |  Packet Type(s)       |  7        || SSI2 & Angular       |
            |                       |           || Rate & Acceleration  |
            +-----------------------+-----------+-----------------------+
            || User Algorithm       |  1        | Algorithm Enabled     |
            || Behavior             |           |                       |
            +-----------------------+-----------+-----------------------+
            |  Orientation          | 0         | +X +Y +Z              |
            +-----------------------+-----------+-----------------------+
            || Termination Resistor | 0         | Resistor Not Enabled  |
            || Enabled              |           |                       |
            +-----------------------+-----------+-----------------------+
            || Baud Rate Detect     | 0         || Baud Rate Detect     |
            || Enabled              |           || Not Enabled          |
            +-----------------------+-----------+-----------------------+
            |  Bank0 PS values      | Set during CAN Task Initialization|
            +-----------------------+-----------------------------------+
            |  Bank1 PS values      | Set during CAN Task Initialization|
            +-----------------------+-----------------------------------+

    *   *CAN task initialization*.  During initialization of the CAN task,  all of the PS values are set to
        predefined values, as described in the following tables:

    .. table:: *User Configuration Data Set Bank0 Pre-defined Values*

            +----------------------+-----------------------------------------------+-----------+
            |  **Bank0 PS**        | **Literal Constant**                          | **Value** |
            +----------------------+-----------------------------------------------+-----------+
            | Algorithm Reset      | SAE_J1939_GROUP_EXTENSION_ALGORITHM_RESET     | 80        |
            +----------------------+-----------------------------------------------+-----------+
            | Save Configuration   | SAE_J1939_GROUP_EXTENSION_SAVE_CONFIGURATION  | 81        |
            +----------------------+-----------------------------------------------+-----------+
            | Hardware BIT         | SAE_J1939_GROUP_EXTENSION_TEST_HARDWARE       | 82        |
            +----------------------+-----------------------------------------------+-----------+
            | Software BIT         | SAE_J1939_GROUP_EXTENSION_TEST_SOFTWARE       | 83        |
            +----------------------+-----------------------------------------------+-----------+
            | Status               | SAE_J1939_GROUP_EXTENSION_TEST_STATUS         | 84        |
            +----------------------+-----------------------------------------------+-----------+

    .. table:: *User Configuration Data Set Bank1 Pre-defined Values*

            +----------------------+-----------------------------------------------+-----------+
            | **Bank1 PS**         | **Literal Constant**                          | **Value** |
            +----------------------+-----------------------------------------------+-----------+
            | Packet Rate          | SAE_J1939_GROUP_EXTENSION_PACKET_RATE         | 85        |
            +----------------------+-----------------------------------------------+-----------+
            | Packet Type(s)       | SAE_J1939_GROUP_EXTENSION_PACKET_TYPE         | 86        |
            +----------------------+-----------------------------------------------+-----------+
            | Filter Cutoff Freqs  | SAE_J1939_GROUP_EXTENSION_DIGITAL_FILTER      | 87        |
            +----------------------+-----------------------------------------------+-----------+
            | Orientation          | SAE_J1939_GROUP_EXTENSION_ORIENTATION         | 88        |
            +----------------------+-----------------------------------------------+-----------+
            | User Behavior        | SAE_J1939_GROUP_EXTENSION_USER_BEHAVIOR       | 89        |
            +----------------------+-----------------------------------------------+-----------+
            | Angle Alarm          | SAE_J1939_GROUP_EXTENSION_ANGLE_ALARM         | 91        |
            +----------------------+-----------------------------------------------+-----------+
            | Cone Alarm           | SAE_J1939_GROUP_EXTENSION_CONE_ALARM          | 92        |
            +----------------------+-----------------------------------------------+-----------+
            | Acceleration         | SAE_J1939_GROUP_EXTENSION_ACCELERATION_PARAM  | 93        |
            +----------------------+-----------------------------------------------+-----------+

    *   *User Configuration Data Set Stored in EEPROM*.  When the application starts and
        the EEPROM user configuration is valid, the user parameters are read from EEPROM and stored in the RAM data structure for user parameters.

    *   *Set Messages*.  Set messages can update parameters in the user configuration data set.  The EEPROM User Configuration data set
        is rewritten when the Save User Configuration Set message is received.
