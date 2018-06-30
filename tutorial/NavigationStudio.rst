Capturing, Displaying, and Saving Data
***************************************

.. contents:: Contents
    :local:

With the algorithm and message created and running on the OpenIMU hardware, the message description
added to *openimu.json*, and *python-openimu* installed on your system, we are ready to collect
data.

OpenIMU Server
===============

To capture data using the Aceinna Navigation Studio, the first step is to start the python-based
server that will capture the serial data streaming over the COM port.  This can be done by
sending the following command at a terminal prompt:

::

    python server.py


This will initiate a search for the OpenIMU device on the machine's COM ports. When detected, the
terminal will return a message similar to the following:

|ServerCapture|

**Figure 1: Server Connection Message at the Terminal Prompt**


Connect to Aceinna Navigation Studio
=====================================

To capture and display the data on the Aceinna Navigation Studio, open a browser to
https://developers.aceinna.com and log in.  From the menu on the left, select Devices, and connect.
The following will appear if connected properly:

|ANS_Connection|

**Figure 2: Connection to IMU Server**


If desired, the packet output rate and other settings can be changed here.


Connect to Aceinna Navigation Studio
=====================================

For a live display of data from the device, select the ‘Record’ menu. An example capture follows:

|ANS_Plot|

**Figure 3: Leveler Angle Data Plot**


Logging Data
=============

To log data select the “Log Control” switch.  The output file consists of the the data in the
serial message.  In particular the message consists of:

    * Time (in counts and seconds)
    * Roll and pitch angles (in degrees)
    * Accelerometer data (in m/s^2)


The following figure shows the contents of the captured data file, indicating that all selected
data are saved as intended.

|ANS_Collect|

**Figure 4: Leveler Angle Data File**



.. Image locations are specified below

.. |ServerCapture| image:: ../media/tutorial/Leveler_ServerCapture.PNG
   :width: 6.5in

.. |ANS_Connection| image:: ../media/tutorial/Leveler_DevelopersPage.PNG
   :width: 8.0in

.. |ANS_Plot| image:: ../media/tutorial/Leveler_AttitudePlot.PNG
   :width: 6.5in

.. |ANS_Collect| image:: ../media/tutorial/Leveler_OutputData.PNG
   :width: 6.5in
