============
Instructions
============

Use strsvr to decode aceinna-user data
======================================

Use strsvr to decode aceinna-user data format. Decode aceinna format data and display information in monitor 
dialog and save it in files.

Set input stream parameter
--------------------------

Select serial for (0) input. Click “opt” button to open the Serial Options dialog.

 .. image:: ../media/serial_options.png
        :height: 200

Select the first serial port in the serial Options dialog.

 .. image:: ../media/first_serial.png
        :height: 200

Bitrate is selected as 460800.

 .. image:: ../media/bitrate.png
        :height: 200

Set output files path
---------------------

Select the path to save the file. For example: C:/\Users/\zhangchen/\Desktop/\rtklog/\.

 .. image:: ../media/path.png
        :height: 200

Show the data in monitor dialog and save file
---------------------------------------------

Click the small square button to open Input Stream Monitor dialog.

 .. image:: ../media/small_square_button.png
        :height: 200

Select the aceinna-user format.

 .. image:: ../media/aceinna_user.png
        :height: 200

Click "start" button to start receiving data.

 .. image:: ../media/start_button.png
        :height: 200

Strsvr is running.

 .. image:: ../media/strsvr_running.png
        :height: 200

The data decoding information is showed in monitor dialog.

 .. image:: ../media/input_stream_monitor.png
        :height: 200

The file is saved in the previous output path.

 .. image:: ../media/output_path.png
        :height: 200

Use RTKLIBNAVI to decode aceinna-user data
==========================================

Aceinna-raw data is the result outputfrom OpenRTK330. Using rtklibnavi to connect the first serial port of 
openrtk330,  the RTK processing result data can be recognized These data can be displayed by SNR plot, sky map 
and GND Trk.

 .. image:: ../media/displayed.png
        :height: 200

Set input stream parameter
--------------------------

Click the ‘I’ button to open Input Streams dialog.

 .. image:: ../media/Ibutton.png
        :height: 200

Check (1) Rover in the Input Streams dialog.

 .. image:: ../media/check_rover.png
        :height: 200

Select "serial" in the type option.

 .. image:: ../media/select_serial.png
        :height: 200

Click "opt" button to open the Serial Options dialog.

 .. image:: ../media/opt_button.png
        :height: 200

Select the frist serial port in the serial Options dialog.

 .. image:: ../media/first_serial2.png
        :height: 200

Bitrate is selected as 460800.

 .. image:: ../media/bitrate2.png
        :height: 200

Format is selected as Aceinna-raw.

 .. image:: ../media/aceinna_raw.png
        :height: 200

Set output log files path
-------------------------

Select the path to save the file. For example: C:/\Users/\zhangchen/\Desktop/\rtklog/\.

Click the ‘L’ button to open Log Streams dialog.

 .. image:: ../media/Lbutton.png
        :height: 200

Check (6) Rover ,select File type and input the log file paths. Click "OK" button.

 .. image:: ../media/OKbutton.png
        :height: 200

Start to receive data
---------------------

Click the "start" button to start receiving the data. 

 .. image:: ../media/start_button2.png
        :height: 200

When receiving the data, the SNR bar is plotted.

 .. image:: ../media/snr_plot.png
        :height: 200

Click the arrow button to switch view (SNR bar, sky map, positioning coordinates, horizontal error scatter, 
position error timeseries in north, east and up).

 .. image:: ../media/arrow_button.png
        :height: 200

The sky map.

 .. image:: ../media/sky_map.png
        :height: 200

Both sky map and SNR plot.

 .. image:: ../media/both_sky_snr.png
        :height: 200

The Gnd Trk.

 .. image:: ../media/gnd_trk.png
        :height: 200

Click the "Plot" button to open RTKPLOT.

 .. image:: ../media/rtkplot.png
        :height: 200

The RTKPLOT dialog.

 .. image:: ../media/rtkplot_dialog.png
        :height: 200

Select the drop-down list to switch views.

 .. image:: ../media/switch_views.png
        :height: 200

The Position views.

 .. image:: ../media/position_views.png
        :height: 200

Click "stop" button to stop receiving data.

 .. image:: ../media/stop_button.png
        :height: 200

The file is saved in the previous output path.

 .. image:: ../media/output_path2.png
        :height: 200

Use RTKLIBNAVI to decode aceinna-raw data
=========================================

Aceinna-raw data contains the original data of rover station and base station. Using rtklibnavi to connect the third serial 
port of openrtk330, the rover station and the base station information can be read at the same time. These data can be displayed by SNR plot, 
sky map, baseline and GND Trk. At the same time, these data can also be used for RTK processing.

 .. image:: ../media/snr_sat_base_trk.png
        :height: 200

Set input stream parameter
--------------------------

Click the ‘I’ button to open Input Streams dialog.

 .. image:: ../media/Ibutton2.png
        :height: 200

Check (1) Rover in the Input Streams dialog.

 .. image:: ../media/check_rover2.png
        :height: 200

Select serial in the type option.

 .. image:: ../media/select_serial2.png
        :height: 200

Click "opt" button to open the Serial Options dialog.

 .. image:: ../media/opt_button2.png
        :height: 200

Select the third serial port in the serial Options dialog.

 .. image:: ../media/third_serial.png
        :height: 200

Bitrate is selected as 460800.

 .. image:: ../media/bitrate3.png
        :height: 200

Format is selected as Aceinna-raw.

 .. image:: ../media/aceinna_raw2.png
        :height: 200

RTK processing config
---------------------

Close the Input Streams dialog and click the "options" button to open the options dialog.

 .. image:: ../media/options_button.png
        :height: 200

In the options dialog, choose the RTK posting mode option as “kinematic” or “static”.

 .. image:: ../media/posting_mode.png
        :height: 200

Start to receive data
---------------------

Click "start" button to start receiving the data.

 .. image:: ../media/start_button3.png
        :height: 200

When receiving the data, the SNR map of Rover and base according to the data will appear in GUI, and RTK results 
will be displayed.

 .. image:: ../media/displayed2.png
        :height: 200

Click the arrow button to switch view (SNR bar, sky map, positioning coordinates, horizontal error scatter, 
position error timeseries in north, east and up).

 .. image:: ../media/arrow_button2.png
        :height: 200

The sky maps.

 .. image:: ../media/sky_map2.png
        :height: 200

The baseline.

 .. image:: ../media/baseline.png
        :height: 200

The Gnd Trk.

 .. image:: ../media/gnd_trk2.png
        :height: 200

Click "Plot" button to Open RTKPLOT.

 .. image:: ../media/rtkplot2.png
        :height: 200

The RTKPLOT dialog.

 .. image:: ../media/rtkplot_dialog2.png
        :height: 200

Select the drop-down list to switch views.

 .. image:: ../media/switch_views2.png
        :height: 200

The Position views.

 .. image:: ../media/position_view.png
        :height: 200