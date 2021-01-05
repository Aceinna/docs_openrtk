========
Overview
========

What is RTKlib
^^^^^^^^^^^^^^

RTKLIB is an open source program package for standard and precise positioning with GNSS (global
navigation satellite system). It supports standard and precise positioning algorithms with GPS, 
GLONASS, Galileo, QZSS, BeiDou and SBAS.

RTKlib tools supporting Aceinna Format
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

RTKlib tools supporting Aceinna Format is s special version of RTKlib which supports aceinna data 
format to display data, decode data, save data, and also plotting and RTK processing.

Aceinna data format
^^^^^^^^^^^^^^^^^^^

Aceinna-user and aceinna-raw are two data formats exported from Openrtk330LI; They are output from 
serial port 1 and serial port 3 of Openrtk330LI; Aceinna-user data include imu raw data, rtk and ins 
solution; Aceinna-raw includes rover, base RTCM data and imu raw data.

Aceinna-User Format
~~~~~~~~~~~~~~~~~~~

**Data format definition**

+---------+---------+--------------+--------------+---------------+--------------+---------+---------+
| Start 1 | Start 2 | Frame type 1 | Frame type 2 | Data length 1 | Data content | Check 1 | Check 2 |
+---------+---------+--------------+--------------+---------------+--------------+---------+---------+

**Description**

- Start: Each frame of data starts with this, 2 bytes: 0x55 0x55.
 - Frame type: 2 bytes, high byte first.
 - Data length: 1 byte, refers to the byte length of the data content.
 - Data content: maximum 255 bytes.
 - Check: crc16 check, 2 bytes, low byte first, bytes from the beginning of the "Frame type" to the end of the "Data content" are included in the check calculation, and the check algorithm C code is as follows:

.. code:: c

 uint16_t CalculateCRC (uint8_t *buf, uint16_t  length)
 {
    uint16_t crc = 0x1D0F;

    for (int i=0; i < length; i++) {
        crc ^= buf[i] << 8;
        for (int j=0; j<8; j++) {
            if (crc & 0x8000) {
                crc = (crc << 1) ^ 0x1021;
            }
            else {
                crc = crc << 1;
            }
        }
    }
    
    return ((crc << 8 ) & 0xFF00) | ((crc >> 8) & 0xFF);
 }

**Frame types**

Aceinna-user has five types of data, namely "S1", "G1", "I1", "O1" and "Y1"; For the specific structure of each type of format, please refer to the openrtk documentation：
https://openrtk.readthedocs.io/en/latest/communication_port/User_uart.html#imu-raw-data-packet

Aceinna-raw Format
~~~~~~~~~~~~~~~~~~

Aceianna-raw is composed of four format types of $GPGGA，$GPIMU，$GPROV，$GPREF;

**$GPGGA**

$GPGGA is the standard GGA format.

**$GPIMU**

$GPIMU is the IMU information in NMEA format.

 +--------+--------------+---------+---------+---------+---------+---------+---------+
 | $GPIMU | time of week | accel-x | accel-y | accel-z | gyro-x  | gyro-y  | gyro-z  |
 +--------+--------------+---------+---------+---------+---------+---------+---------+
 
**$GPROV**

$GPROV contains the RTCM package from Rover.

 +--------+--------------+-------------+----------+----------+
 | $GPROV | time of week | left length | RTCM bin |          |
 +--------+--------------+-------------+----------+----------+

**$GPREF**

$GPREF contains the RTCM package from Base.

 +--------+--------------+-------------+----------+----------+
 | $GPRRF | time of week | left length | RTCM bin |          |
 +--------+--------------+-------------+----------+----------+
