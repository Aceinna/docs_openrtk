NMEA
----

**$GNGGA**

Format: $GNGGA,<1>,<2>,<3>,<4>,<5>,<6>,<7>,<8>,<9>,M,<10>,M,< 11>,<12>*xx<CR><LF>
E.g:
$GNGGA,072446.00,3130.5226316,N,12024.0937010,E,4,27,0.5,31.924,M,0.000,M,2.0,*44
Field explanation:
 - **<0>** $GNGGA
 - **<1>** UTC time, the format is hhmmss.sss
 - **<2>** Latitude, the format is ddmm.mmmmmmm
 - **<3>** Latitude hemisphere, N or S (north latitude or south latitude)
 - **<4>** Longitude, the format is dddmm.mmmmmmm
 - **<5>** Longitude hemisphere, E or W (east longitude or west longitude)
 - **<6>** GNSS positioning status: 0 not positioned, 1 single point positioning, 2 differential GPS fixed solution, 4 fixed solution, 5 floating point solution
 - **<7>** Number of satellites used
 - **<8>** HDOP level precision factor
 - **<9>** Altitude
 - **<10>** The height of the earth ellipsoid relative to the geoid
 - **<11>** Differential time
 - **<12>** Differential reference base station label
 - **\*** Statement end marker
 - **xx** XOR check value of all bytes starting from $ to \*
 - **<CR>** Carriage return, end tag
 - **<LF>** line feed, end tag

**$GNRMC**

Format: $GNRMC,<1>,<2>,<3>,<4>,<5>,<6>,<7>,<8>,<9>,<10>,<11>,< 12>*xx<CR><LF>
E.g:
$GNRMC,072446.00,A,3130.5226316,N,12024.0937010,E,0.01,0.00,040620,0.0,E,D*3D
Field explanation:
 - **<0>** $GNRMC
 - **<1>** UTC time, the format is hhmmss.sss
 - **<2>** Positioning status, A=effective positioning, V=invalid positioning
 - **<3>** Latitude, the format is ddmm.mmmmmmm
 - **<4>** Latitude hemisphere, N or S (north latitude or south latitude)
 - **<5>** Longitude, the format is dddmm.mmmmmmm
 - **<6>** Longitude hemisphere, E or W (east longitude or west longitude)
 - **<7>** Ground speed
 - **<8>** Ground heading (take true north as the reference datum)
 - **<9>** UTC date, the format is ddmmyy (day, month, year)
 - **<10>** Magnetic declination (000.0~180.0 degrees)
 - **<11>** Magnetic declination direction, E (east) or W (west)
 - **<12>** Mode indication (A=autonomous positioning, D=differential, E=estimation, N=invalid data)
 - **\*** Statement end marker
 - **XX** XOR check value of all bytes starting from $ to *
 - **<CR>** Carriage return, end tag
 - **<LF>** line feed, end tag

**$GNGSA**

format:
$GNGSA,<1>,<2>,<3>,<3>,,,,,<3>,<3>,<3>,<4>,<5>,<6>,<7> *xx<CR><LF>
E.g:
$GNGSA,A,3,03,06,09,17,19,23,28,,,,,,3.0,1.5,2.6,1*25
$GNGSA,A,3,65,66,67,81,82,88,,,,,,,2.4,1.3,2.1,2*36
$GNGSA,A,3,02,05,09,15,27,,,,,,,,,10.8,2.7,10.4,3*3A
$GNGSA,A,3,01,02,07,08,10,13,27,28,32,33,37,,2.1,1.0,1.9,5*33
Field explanation:
 - **<1>** Mode: M=Manual, A=Auto
 - **<2>** Positioning type: 1=not positioned, 2=two-dimensional positioning, 3=three-dimensional positioning
 - **<3>** PRN code (Pseudo Random Noise Code), channels 1 to 12, up to 12
 - **<4>** PDOP position precision factor
 - **<5>** HDOP level precision factor
 - **<6>** VDOP vertical precision factor
 - **<7>** GNSS system ID: 1(GPS), 2(GLONASS), 3(GALILEO), 5(BEIDOU)
 - **\***  Statement end marker
 - **xx** XOR check value of all bytes starting from $ to *
 - **<CR>** Carriage return, end tag
 - **<LF>** line feed, end tag