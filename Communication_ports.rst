Communication Ports and Operation
---------------------------------

USER UART has serial port IAP (program firmware APP) function, ST UART PROG serial port is 
the serial port for SDK firmware programming, users must connect. BT UART and ETH can pull 
base rtcm3 for RTK operation, users need to choose at least one connection. Other interface 
users can connect according to their needs. The hardware design can refer to OpenRTK330 EVK.


User Port
~~~~~~~~~

- **Pin**: USER_UART_RX(#55), USER_UART_TX(#56)
- **Default configuration**
 
 - Baud tare: 460800 b/s
 - Stop bit: 1
 - Data bits: 8
 - Check Digit: None
 
- **Data format**: ACEINNA format, NMEA format
- **The main function**

 - Obtain module information: hardware version number, software version number;
 - Obtain and configure module user parameters;
 - Send data packets: IMU raw data, positioning data, satellite data;
 - Send NMEA format data;

- **Function details**
 The following takes configuration parameters as an example to introduce how to use the ACEINNA format:

 1) Send the "gA" command to the module to obtain all current user parameters:
  **gA command**: [0x55, 0x55, 0x67, 0x41, 0, 0x31, 0x0A]
 2) Use the "uP" command to modify the  parameters:
  **uP command**: [0x55, 0x55, 0x75, 0x50, data length, parameter number, parameter value, CRC_L, CRC_H]
  
  For example: configure the three parameters of leverArmBx, leverArmBy, leverArmBz to
  [0.5, -0.5, 1] ​​(unit m), you need to send the "uP" command three times, and the setting result will 
  be returned each time. After the last setting result is returned, send again Set the command next time.
  
  - **Configure leverArmBx**: [0x55, 0x55, 0x75, 0x50, 0x08, 0x04, 0, 0, 0, 0, 0, 0, 0x3F, 0x1D, 0x32]
  - **Configure leverArmBy**: [0x55, 0x55, 0x75, 0x50, 0x08, 0x05, 0, 0, 0, 0, 0, 0, 0xBF, 0xCB, 0x69]
  - **Configure leverArmBz**: [0x55, 0x55, 0x75, 0x50, 0x08, 0x06, 0, 0, 0, 0, 0, 0x80, 0x3F, 0x89, 0x0C]

 3) Use the "sC" command to save the parameter value:
  **sC command**: [0x55, 0x55, 0x73, 0x43, 0, 0xC8, 0xCB]

Special Note
""""""""""""

.. code:: c

 "initial":{
    "useDefaultUart": 1,
       "uart":[
        {
            "name": "GNSS",	        
            "value": "com10",
            "enable": 1 
        },
        {
            "name": "DEBUG",
            "value": "com11",
            "enable": 1 
        }
     ],
    "userParameters": [
        {
            "paramId": 4,
            "name": "lever arm x",
            "value": 0.0
        },
        {
            "paramId": 5,
            "name": "lever arm y",
            "value": 0.0
        }
     ]
  }

The user serial port is the serial port connected by the python driver. If the user needs to enable the 
data log function or automatically configure user parameters when the python driver is started, first 
configure the "initial" field in openrtk.json as shown in Figure above.

**Use OpenRTK/OpenIMU python driver operation**

1) Set the log serial port

When the python driver is started with the "-r" suffix, the log function will be enabled and the data of 
the three serial ports of USER, GNSS and DEBUG will be recorded at the same time. The USER serial port 
number can be automatically identified by the python driver, but GNSS and DEBUG cannot. The user must 
set these two serial port numbers.

*Case 1*: The GNSS/DEBUG of OpenRTK330 EVK is the USER serial port number plus 1 and 2 respectively. 
Just configure the "useDefaultUart" field to 1, and the "uart" field does not work at this time.

.. image:: media/Fig_27_Micro_USB.png

*Case 2*: If the user needs to specify the GNSS/DEBUG serial port number, or does not use the GNSS/DEBUG 
serial port (the user has not made a hardware connection), the "useDefaultUart" needs to be configured 
to 0, and the "uart" field is valid at this time, the GNSS/DEBUG When "enable" is 1, it means to use 
this serial port. When not in use, configure it to 0. "Value" should be the serial port name of the 
serial port in the system. For example: under windos, open the device manager, as shown in Figure above, 
find the connected GNSS and DEBUG serial numbers are COM10 and COM11 respectively, the configuration 
should be as follows:

.. code:: c

 "uart":[
            {
                "name": "GNSS",
                "value": "com10",
                "enable": 1 
            },
            {
                "name": "DEBUG",
                "value": "com11",
                "enable": 1 
            }
        ],


2) Setting paracmeters

When starting the python driver with the "-s" suffix, the "userParameters" parameters can be automatically 
configured to the OpenRTK device and saved after power off. Find "userParameters" as shown in Figure 2, 
and configure fields for user parameters. All configurable fields are in "userConfiguration", except for 
"Data CRC" and "Data Size" whose paramId is 0 or 1 are not configurable, the others can be added to 
"userParameters". Among them, "paramId" and "value" are mandatory fields, the value of paramId must be 
consistent with that in "userConfiguration", and the type of value must be consistent with "type".

For example: to configure Ethernet and NTRIP services, the following configuration is required, where the 
Ethnet mode value is 1 to use static IP mode, and the value is 0 to use DHCP mode.

.. code:: c

        "userParameters": [
            {
                "paramId": 13,
                "name": "Ethnet mode",
                "value": 1
            },
            {
                "paramId": 14,
                "name": "STATIC IP",
                "value": "192.168.137.110"
            },
            {
                "paramId": 15,
                "name": "NETMASK",
                "value": "255.255.255.0"
            },
            {
                "paramId": 16,
                "name": "GATEWAY",
                "value": "192.168.137.1"
            },
            {
                "paramId": 18,
                "name": "IP",
                "value": "203.107.45.154"
            },
            {
                "paramId": 19,
                "name": "PORT",
                "value": 8001
            },
            {
                "paramId": 20,
                "name": "MOUNT POINT",
                "value": " RTCM32_GGB"
            },
            {
                "paramId": 21,
                "name": "USER NAME",
                "value": "username"
            },
            {
                "paramId": 22,
                "name": "PASSWORD",
                "value": "password"
            }
        ] 


ST GNSS UART1
~~~~~~~~~~~~~

- **Pin**: ST_UART1_TX(#59), ST_UART1_RX(#60)
- **Default configuration**

 - Baud tare: 460800 b/s
 - Stop bit: 1
 - Data bits: 8
 - Check Digit: None

- **Data formation**: RTCM3 format
- **Main function**: Send raw data of GNSS receiver satellite signal

DEBUG UART1
~~~~~~~~~~~

- **Pin**: DEBUG_TX(#51), DEBUG_RX(#52)
- **Default configuration**

 - Baud tare: 460800 b/s
 - Stop bit: 1
 - Data bits: 8
 - Check Digit: None

- **Data formation**: ASSIC format, "P1" packet format
- **Main function**: 
 
 - Send "p1" packet data (more detailed than user serial port data), not sending by default
 - Get user parameters (only basic parameters are included, user serial port can get all parameters)
 - Control "p1" packet data on or off

ST UART PROG
~~~~~~~~~~~~

- **Pin**: ST_UART_PROG_TX(#49), ST_UART1_PROG_RX(#50)
- **Default configuration**

 - Baud tare: 460800 b/s
 - Stop bit: 1
 - Data bits: 8
 - Check Digit: None

- **Main function**: ST GNSS chip firmware download interface (SDK download port)

BT UART
~~~~~~~

- **Pin**: BT_UART2_RX(#35), BT_UART2_TX(#36)
- **Default configuration**

 - Baud tare: 460800 b/s
 - Stop bit: 1
 - Data bits: 8
 - Check Digit: None

- **Main function**

 - Receive RTCM3 data from GNSS base station
 -  Send module position data in NMEA GPGGA format

SPI Pin Definition
~~~~~~~~~~~~~~~~~~

- **Pin**: USER_MOSI(#29), USER_SCK(#30), USER_NSS(#31), USER_MISO(#32)
- **Default configuration**

 - Frame format: Motorola
 - Data length: 8 bits
 - First bit: 1
 - CPOL: High
 - CPHA: 2Edge

- **Main function**

 - Send "p1" data, "p1" packet format (see 4.3 for details)

CAN Pin Definition
~~~~~~~~~~~~~~~~~~

- **Pin**: CAN_RX(#53), CAN_TX(#54)
- **Default configuration**

 - ECU address: 128 (automatically match, add 1 to this address, maximum 247)
 - Baud rate: 250K

- **Data format**: can communication protocol, which can be divided into the following 3 categories 
  according to functions:

 - Setting parameters: the user sends a setting parameter command, the module does not return
 - Get parameters: the user sends a get parameter command, the content of the command is the PF number 
   and PS number of the data required by the user, and the module returns the corresponding data frame
 - Data packet: The module continuously sends data packets according to the data type and frequency 
   configured by the user

- **Main function**

 - Support SAE J1939 protocol
 - Configure CAN interface parameters
 - Send user data packet

RMII Pin Definition
~~~~~~~~~~~~~~~~~~~

- **Pin**: ETH_RESET(#8), RMII_TXD0(#9), RMII_TXD1(#10), RMII_TX_EN(#11), VDD_CORE(#12), VIN(#13), RMII_RXD1(#14), 
ETH_MDC(#15),RMII_RXD0(#16),RMII_REF_CLK(#17),ETH_MDIO(#18),RMII_CRS_DV(#19)

- **Default configuration**

 - DHCP mode
 - Hostname: openrtk,  you can access the Web Interface through http://openrtk in the LAN

- **Main function**

 - Support static IP mode and DHCP mode
 - Access to Web Interface configuration parameters (including Ethernet, NTRIP, etc.)
 - Establish NTRIP CLIENT to pull base rtcm3 data

