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