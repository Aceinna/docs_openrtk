
RAWDATA App
=============
 
 The RAWDATA App is a set of open source firmware of OpenRTK330 module/EVB, and is prepared for users who has their own GNSS RTK and INS positioing engine and wants to embedd their positioning engine in the App to build a GNSS/INS integrated positioning system.


Build
~~~~~~~

 Open VS Code and go to "Aceinna" extension home page, if for a new project, click "Custom IMU Examples" and find "OpenRTK330LI/RAWDATA" from the example dropdown list, click "Import" to create a new project with source code on your PC; if the import has been done before, go to the bottom "Recent Projects" and click "Open" on the right side

     .. figure:: ../media/rawdata_App_import.jpg
        :width: 6.0in 
        :height: 4.5in


 The App then starts pull `OpenRTK330 open source firmware lib <https://github.com/Aceinna/openRTK330-lib.git>`_ and copys the entire library into the App's current project folder under ".pio"

     .. figure:: ../media/rawdata_build.png
        :width: 6.0in 
        :height: 4.5in

 As shown by the above figure, if the downloading process finishes, you will have the whole "RAWDATA App" workspace to build, edit, and download firmware to OpenRTK330 module. 

 The red marked #1, #2, and #3 on the bottom toolbar are the shortcuts to return to "Aceinna" extension "Home" page, "Build" firmware, and "Upload" firmware commands, respectively. Click the "Build" to build the entire project, and if without errors, click the "Upload" icon to flash the "RAWDATA"App to OpenRTK330 module. 
 

Algorithm Code Interfaces
~~~~~~~~~~~~~~~~~
 This section describes the detail code interfaces for user to briefly understand the firmware frame and to embedd their own algorithms quickly. It's recommended to refer to the `OpenRTK330 module hardware <https://openrtk.readthedocs.io/en/latest/OpenRTK330.html>`_ and `EVB hardware and layout <https://openrtk.readthedocs.io/en/latest/EVB-OpenRTK330LI/schematic.html>`_ for understanding of the firmware here.

 Checking the "main.c" source file, the whole system is initialized here, and the four UART ports of the MCU are initialzed with a baud rate of 460800 per seconds and are used as the system's primary I/O. Most importantly, there are four RTOS tasks created as the follows:

 * IMU Data Acquisition Task - *DACQ_TASK*, highest priority
   
   * Function: acquires raw IMU data in user configurable data rate (default 100 Hz)
   * Source file: *taskDataAcquisition.c*
   * Algorithm interface: user can put INS algorithm entry in the place as shown by the figure below 

        .. figure:: ../media/ins_algo_entry.jpg
            :width: 5.0 in
            :height: 3.0 in

 * GNSS Data Acquisition Task - *GPS_TASK*

   * Function: acquires GNSS RTCM data and get it decoded to GNSS observation and ephemeris structs
   * Source file: *taskGnssDataAcq.c*
   * Algorithm interface: N/A

 * GNSS RTK Algorithm Task - *RTK_TASK*
    
   * Function: gets GNSS observation and ephemeris data from *GPS_TASK* and fullfill RTK algorithm
   * Source file: *taskRTK.c*
   * Algorithm interface: GNSS RTK algorithm entry as shown by the figure below. Besides, two options for NMEA GGA up to NTRIP server are provided with void messages, user has to fill the GGA message with position that is required to pull GNSS correction data from NTRIP server to complete the RTK data loop. 

        .. figure:: ../media/rtk_algo_entry.jpg
            :width: 6.0 in
            :height: 3.0 in

 * Ethernet Connection Task - *ETH_TASK*

   * Function: provides ethernet driver for internet connectivity
   * Source file: *taskETH.c*
   * Algorithm interface: N/A
