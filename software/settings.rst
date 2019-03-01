Settings Modules
================

.. contents:: Contents
    :local:

USER Configuration.
------------------------------

Configuration parameters in EEPROM 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

OpenIMU software framework provides possibility for user to store arbitrary configuration parameters
in nonvolatile EEPROM. These parameters will be validated and applied to system upon reset or power-up.
Parameters which passed validation will override default factory settings.
User EEPROM has size 16KB. Each parameter in user EEPROM has size 8 bytes (64-bit word), so user EEPROM
can contain up to 2K parameters. If desired one can use few consequtive parameters to store arbitrary 
value or data structure. One parameter is good for a value of double or long long type. Also it can be
considered as 8 bytes of arbitrary data (string or array). There are few pre-allocated recommended 
parameters which can be useful while working with OpenIMU software framework. Initial definition of
parameters structure located in file UserConfiguration.h. New arbitrary parameters are welcome. 

::

    /// User defined configuration strucrture
    ///Please notice, that parameters are 64 bit to accomodate double types as well as string or byte array types

    typedef struct {
        uint64_t           dataCRC;             /// CRC of user configuration structure CRC-16
        uint64_t           dataSize;            /// Size of the user configuration structure 
        
        int64_t            userUartBaudRate;    /// baudrate of user UART, bps. 
                                                /// valid options are:
                                                /// 4800
                                                /// 9600
                                                /// 19200
                                                /// 38400
                                                /// 57600
                                                /// 115200
                                                /// 230400
                                                /// 460800
        uint8_t            userPacketType[8];   /// User packet to be continiously sent by unit
                                                /// Packet types defined in structure UserOutPacketType
                                                /// in file UserMessaging.h
                                                
        int64_t            userPacketRate;      /// Packet rate for continiously output packet, Hz.
                                                /// Valid settings are: 0 ,2, 5, 10, 20, 25, 50, 100, 200 

        int64_t            lpfAccelFilterFreq;  /// built-in lpf filter cutoff frequency selection for accelerometers   
        int64_t            lpfRateFilterFreq;   /// built-in lpf filter cutoff frequency selection for rate sensors   
                                                /// Options are:
                                                /// 0  -  Filter turned off
                                                /// 50 -  Butterworth LPF 50HZ
                                                /// 20 -  Butterworth LPF 20HZ
                                                /// 10 -  Butterworth LPF 10HZ
                                                /// 05 -  Butterworth LPF 5HZ
                                                /// 02 -  Butterworth LPF 2HZ
                                                /// 25 -  Butterworth LPF 25HZ
                                                /// 40 -  Butterworth LPF 40HZ
        
        uint8_t           orientation[8];       
	                                        /// unit orientation as string 
                                                /// "SFSRSD"
                                                ///  Where S is sign (+ or -)
                                                ///  F - forward axis (X ot Y or Z)
                                                ///  R - right axis (X ot Y or Z)
                                                ///  D - down axis (X ot Y or Z)
						///  For example "+X+Y+Z"	
        
        //***************************************************************************************
        // here is the border between arbitrary parameters and platform configuration parameters
        //***************************************************************************************

        // place new arbitrary configuration parameters here
        // parameter size should even to 8 bytes
        // Add parameter offset in UserConfigParamOffset structure if validation or
        // special processing required 

    } UserConfigurationStruct;

Default configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Default system parameters reside in the **gDefaultUserConfig** structure in file **UserConfiguration.c**.
They are becoming active each time new application image is loaded to the unit or upon reception of the "rD" command. 


Mapping different values into 64-bit parameter
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Below provided recommended mapping of the values of different types into 64-bit parameter.
The mapping though can be arbitrary and in that case should be processed accordingly. 

1. Mapping of 4-byte integer into 64-bit parameter (value is in Little Endian format)
 
       +-----+----+----+-----+----+----+----+----+
       |  0  | 1  | 2  |  3  |  4 |  5 |  6 | 7  | 
       +-----+----+----+-----+----+----+----+----+
       | LSB |    |    | MSB |  0 | 0  | 0  | 0  | 
       +-----+----+----+-----+----+----+----+----+
 
2. Mapping of 2-byte integer into 64-bit parameter (value is in Little Endian format)
 
       +-----+----+----+-----+----+----+----+----+
       |  0  | 1  | 2  |  3  |  4 |  5 |  6 | 7  | 
       +-----+----+----+-----+----+----+----+----+
       | LSB | MSB| 0  |  0  |  0 | 0  | 0  | 0  | 
       +-----+----+----+-----+----+----+----+----+
 
3. Mapping of 4-byte floating point value into 64-bit parameter (value is in Little Endian format)
 
       +-----+----+----+-----+----+----+----+----+
       |  0  | 1  | 2  |  3  |  4 |  5 |  6 | 7  | 
       +-----+----+----+-----+----+----+----+----+
       | LSB |    |    | MSB |  0 | 0  | 0  | 0  | 
       +-----+----+----+-----+----+----+----+----+
 
4. Mapping of 8-byte double value into 64-bit parameter (value is in Little Endian format)
 
       +-----+----+----+-----+----+----+----+----+
       |  0  | 1  | 2  |  3  |  4 |  5 |  6 | 7  | 
       +-----+----+----+-----+----+----+----+----+
       | LSB |    |    |     |    |    |    | MSB| 
       +-----+----+----+-----+----+----+----+----+
 
5. Mapping byte array or string into 64-bit parameter
 
Byte (character) indexes match offset in the 64-bit parameter

       +-----+----+----+-----+----+----+----+----+
       |  0  | 1  | 2  |  3  |  4 |  5 |  6 | 7  | 
       +-----+----+----+-----+----+----+----+----+
 
Adding new parameter
~~~~~~~~~~~~~~~~~~~~~~~~~~

One can arbitrary add new configuration parameters. The steps are:

1. Add required parameter into the **UserConfigurationStruct** in the file **UserConfiguration.h** after system parameters �border� (see above).

2. Add new configuration parameter enumerator into UserConfigParamOffset in the file UserConfiguration.h after USER_LAST_SYSTEM_PARAM. 

3. Add default value of new parameter into structure gDefaultUserConfig in file UserConfiguration.c (if desired)

4. Add validation of new parameter into function UpdateUserParameter (if desired) or explicitly use parameter at your discretion

Changing configuration parameters.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Configuration parameters can be changed any time by sending specific commands (messages) to the unit (("uP" "uA" "uC"). 
Upon reception of corresponding message parameters are validated (if desired), placed into gUserConfiguration structure
and applied to the unit (if desired). See section Messaging Modules for details. Updated parameters will last untill unit
reset or power cycle.
  
Retrieving configuration parameters.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Configuration parameters can be read from unit any time by sending commands "gC" "gP" or "gA" (see messaging-modules).

Saving configuration parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If desired, updated parameters can be saved into EEPROM and will be permanently active untill changed. It can be achieved by sending "sC"
command to the unit. Upon reception of this command gUserConfiguration structure saved into EEPROM.

Restoring default configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If desired, default configuration can be restored and saved into EEPROM. It can be achieved by sending command "rD" to the unit.


