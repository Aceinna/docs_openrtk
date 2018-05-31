Appendix B: NMEA Message Format
*******************************

The GPS receiver outputs data in NMEA-0183 format at 9600 Baud, 8 bits,
no parity bit, and 1 stop bit. The GGA and RMC message packet formats
are explained in this section.

GGA -GPS fix Data
-----------------

Time and position, together with GPS fixing related data (number of
satellites in use, and the resulting HDOP, age of differential data if
in use, etc.).

$GPGGA,hhmmss.ss,Latitude,N,Longitude,E,FS,NoSV,HDOP,msl,m,Altref,m,DiffAge,DiffStation*cs<CR><LF>

+-----------------+-----------------+-----------------+-----------------+
| **Name**        | **ASCII         | **Description** |                 |
|                 | String**        |                 |                 |
+-----------------+-----------------+-----------------+-----------------+
|                 | **Format**      | **Example**     |                 |
+-----------------+-----------------+-----------------+-----------------+
| $GPGGA          | string          | $GPGGA          | Message ID:     |
|                 |                 |                 |                 |
|                 |                 |                 | GGA protocol    |
|                 |                 |                 | header          |
+-----------------+-----------------+-----------------+-----------------+
| hhmmss.ss       | hhmmss.sss      | 092725.00       | UTC Time:       |
|                 |                 |                 | Current time    |
+-----------------+-----------------+-----------------+-----------------+
| Latitude        | dddmm.mmmm      | 4717.11399      | Latitude:       |
|                 |                 |                 | Degrees +       |
|                 |                 |                 | minutes         |
+-----------------+-----------------+-----------------+-----------------+
| N               | character       | N               | N/S Indicator:  |
|                 |                 |                 |                 |
|                 |                 |                 | N=north or      |
|                 |                 |                 | S=south         |
+-----------------+-----------------+-----------------+-----------------+
| Longitude       | dddmm.mmmm      | 00833.91590     | Longitude:      |
|                 |                 |                 |                 |
|                 |                 |                 | Degrees +       |
|                 |                 |                 | minutes         |
+-----------------+-----------------+-----------------+-----------------+
| E               | character       | E               | E/W indicator:  |
|                 |                 |                 |                 |
|                 |                 |                 | E=east or       |
|                 |                 |                 | W=west          |
+-----------------+-----------------+-----------------+-----------------+
| FS              | 1 digit         | 1               | Position Fix    |
|                 |                 |                 | Indicator       |
|                 |                 |                 |                 |
|                 |                 |                 | (See Table      |
|                 |                 |                 | below)          |
+-----------------+-----------------+-----------------+-----------------+
| NoSV            | numeric         | 8               | Satellites      |
|                 |                 |                 | Used:           |
|                 |                 |                 |                 |
|                 |                 |                 | Range 0 to 12   |
+-----------------+-----------------+-----------------+-----------------+
| HDOP            | numeric         | 1.01            | HDOP:           |
|                 |                 |                 | Horizontal      |
|                 |                 |                 | Dilution of     |
|                 |                 |                 | Precision       |
+-----------------+-----------------+-----------------+-----------------+
| msl             | numeric         | 499.6           | MSL Altitude    |
|                 |                 |                 | (m)             |
+-----------------+-----------------+-----------------+-----------------+
| m               | character       | M               | Units: Meters   |
|                 |                 |                 | (fixed field)   |
+-----------------+-----------------+-----------------+-----------------+
| Altref          | blank           | 48.0            | Geoid           |
|                 |                 |                 | Separation (m)  |
+-----------------+-----------------+-----------------+-----------------+
| m               | blank           | M               | Units: Meters   |
|                 |                 |                 | (fixed field)   |
+-----------------+-----------------+-----------------+-----------------+
| DiffAge         | numeric         |                 | Age of          |
|                 |                 |                 | Differential    |
|                 |                 |                 | Corrections     |
|                 |                 |                 | (sec):          |
|                 |                 |                 |                 |
|                 |                 |                 | Blank (Null)    |
|                 |                 |                 | fields when     |
|                 |                 |                 | DGPS is not     |
|                 |                 |                 | used            |
+-----------------+-----------------+-----------------+-----------------+
| DiffStation     | numeric         | 0               | Diff. Reference |
|                 |                 |                 | Station ID      |
+-----------------+-----------------+-----------------+-----------------+
| cs              | hexadecimal     | \*5B            | Checksum        |
+-----------------+-----------------+-----------------+-----------------+
| <CR> <LF>       |                 |                 |  End of message |
+-----------------+-----------------+-----------------+-----------------+
|                 |                 |                 |                 |
+-----------------+-----------------+-----------------+-----------------+

+-----------------+----------------------+
| **Fix Status**  | **Description**      |
+-----------------+----------------------+
| 0               | No fix / Invalid     |
+-----------------+----------------------+
| 1               | Standard GPS (2D/3D) |
+-----------------+----------------------+
| 2               | Differential GPS     |
+-----------------+----------------------+
| 6               | Estimated (DR) Fix   |
+-----------------+----------------------+

..

