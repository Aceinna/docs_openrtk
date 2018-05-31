Appendix A: Installation and Operation of NAV-VIEW
**************************************************

NAV-VIEW has been designed to allow users to control all aspects of the
DMUx81ZA Series operation including data recording, configuration and
data transfer. For the first time, you will be able to control the
orientation of the unit, sampling rate, packet type, hard iron
calibration and filter settings through NAV-VIEW. For proper use with
the DMUx81ZA family version 3.5.2 or higher of NAV-VIEW is required.

Nav-View Computer Requirements
------------------------------

The following are minimum requirements for the installation of the
NAV-VIEW Software:

• CPU: Pentium-class (1.5GHz minimum)

• RAM Memory: 500MB minimum, 1GB+ recommended

• Hard Drive Free Memory: 20MB

• Operating System: Windows 7, 10 

• Properly installed Microsoft .NET 2.0 or higher

Install Nav-View
----------------

To install NAV-VIEW onto your computer:
    A USB to UART Adapter is needed to connect the DMUx81ZA 
    Series to a PC.  To enable UART communications, Pin 7 (DRDY) 
    must be pulled low at startup. An Evaluation kit is available 
    which enables direct connection to a USB port on a PC.

    1. Insert the CD “Inertial Systems Product Support” (Part No.
    8160-0063) in the CD-ROM drive.

    2. Locate the “NAV-VIEW” folder. Double click on the “setup.exe”
    file.

    3. Follow the setup wizard instructions. You will install NAV-VIEW
    and .NET 2.0 framework.

    .. _connections:

Connections
-----------
    DMUx81ZA is shipped with a 6-pin TTL-to-232R cable used for connection 
    between unit and a PC’s USB port. 

    1. Hook up this cable between UART port on DMUx81ZA’s demo board and 
    PC’s USB port.

    2. The input voltage is 3.3VDC and maximum current is draw of 350 mA. 

    3. Wait around 60 seconds during the initialization of DMUx81ZA. 

|warning1| **WARNING**

**Do not reverse the power leads!** Reversing the power leads to the
DMUx81ZA Series can damage the unit; although there is reverse power
protection, ACEINNA is not responsible for resulting damage to the unit
should the reverse voltage protection electronics fail.

Setting up NAV-VIEW
-------------------

With the DMUx81ZA Series product powered up and connected to your PC
serial port, open the NAV-VIEW software application.

    1. NAV-VIEW should automatically detect the DMUx81ZA Series product
    and display the serial number and firmware version if it is
    connected.

    2. If NAV-VIEW does not connect, check that you have the correct COM
    port selected. You will find this under the “Setup” menu. Select the
    appropriate COM port and allow the unit to automatically match the
    baud rate by leaving the “Auto: match baud rate” selection marked.

    3. If the status indicator at the bottom is green and states,
    |UnitConnected|, you’re ready to go. If the status indicator doesn’t
    say connected and is red, check the connections between the DMUx81ZA
    Series product and the computer, check the power supply, and verify
    that the COM port is not occupied by another device.

    4. Under the “View” menu you have several choices of data
    presentation. Graph display is the default setting and will provide
    a real time graph of all the DMUx81ZA Series data. The remaining
    choices will be discussed in the following pages.

Data Recording
--------------

NAV-VIEW allows the user to log data to a text file (.txt) using the
simple interface at the top of the screen. Customers can now tailor the
type of data, rate of logging and can even establish predetermined
recording lengths.

To begin logging data follow the steps below (See `Figure 20 <\l>`__):

Locate the |Folder|\ icon at the top of the page or select “Log to File”
from the “File” drop down menu.

The following menu will appear.

    |LogFile|

Figure 20 Log to File Dialog Screen

Select the “Browse” box to enter the file name and location that you
wish to save your data to.

Select the type of data you wish to record. “Engineering Data” records
the converted values provided from the system in engineering units, “Hex
Data” provides the raw hex values separated into columns displaying the
value, and the “Raw Packets” will simply record the raw hex strings as
they are sent from the unit.

Users can also select a predetermined “Test Duration” from the menu.
Using the arrows, simply select the duration of your data recording.

Logging Rate can also be adjusted using the features on the right side
of the menu.

Once you have completed the customization of your data recording, you
will be returned to the main screen where you can start the recording
process using the |RecordButton| button at the top of the page or select
“Start Logging” from the “File” menu. Stopping the data recording can be
accomplished using the |stop-button| button and the recording can also
be paused using the |Pause-button| button.

Data Playback
-------------

In addition to data recording, NAV-VIEW allows the user to replay saved
data that has been stored in a log file.

To playback data, select “Playback Mode” from the “Data Source” drop
down menu at the top. |Data-Source|

Selecting Playback mode will open a text prompt which will allow users
to specify the location of the file they wish to play back. All three
file formats are supported (Engineering, Hex, and Raw) for playback. In
addition, each time recording is stopped/started a new section is
created. These sections can be individually played back by using the
drop down menu and associated VCR controls.

Once the file is selected, users can utilize the VCR style controls at
the top of the page to start, stop, and pause the playback of the data.

NAV-VIEW also provides users with the ability to alter the start time
for data playback. Using the |Slidebar| slide bar at the top of the page
users can adjust the starting time.

Raw Data Console
----------------

NAV-VIEW offers some unique debugging tools that may assist programmers
in the development process. One such tool is the Raw Data Console. From
the “View” drop down menu, simply select the “Raw Data Console”. This
console provides users with a simple display of the packets that have
been transmitted to the unit (Tx) and the messages received (Rx). An
example is provided in `Figure 21 <\l>`__.

|RawDataConsole|

Figure 21 Raw Data Console

Horizon and Compass View
------------------------

If the DMUx81ZA Series product you have connected is capable of
providing heading and angle information (see `Table 2 <\l>`__), NAV-VIEW
can provide a compass and a simulated artificial horizon view. To
activate these views, simply select “Horizon View” and/or “Compass View”
from the “View” drop down menu at the top of the page (See `Figure
22 <\l>`__).

|CompassView|

Figure 22 Horizon and Compass View

Packet Statistics View
----------------------

Packet statistics can be obtained from the “View” menu by selecting the
“Packet Statistics” option (See `Figure 23 <\l>`__). This view simply
provides the user with a short list of vital statistics (including
Packet Rate, CRC Failures, and overall Elapsed Time) that are calculated
over a one second window. This tool should be used to gather information
regarding the overall health of the user configuration. Incorrectly
configured communication settings can result in a large number of CRC
Failures and poor data transfer.

|PacketStatistics|

Figure 23 Packet Statistics

Unit Configuration
------------------

The Unit Configuration window (See `Figure 24 <\l>`__) gives the user
the ability to view and alter the system settings. This window is
accessed through the “Unit Configuration” menu item under the
configuration menu. Under the “General” tab, users have the ability to
verify the current configuration by selecting the “Get All Values”
button. This button simply provides users with the currently set
configuration of the unit and displays the values in the left column of
boxes.

There are three tabs within the “Unit Configuration” menu; General,
Advanced and BIT Configuration. The General tab displays some of the
most commonly used settings. The Advanced and BIT Configuration menus
provide users with more detailed setting information that they can
tailor to meet their specific needs.

To alter a setting, simply select the check box on the left of the value
that you wish to modify and then select the value using the drop down
menu on the right side. Once you have selected the appropriate value,
these settings can be set temporarily or permanently (a software reset
or power cycle is required for the changes to take affect) by selecting
from the choices at the bottom of the dialog box. Once the settings have
been altered a “Success” box will appear at the bottom of the page.

**IMPORTANT**

Caution must be taken to ensure that the settings selected are
compatible with the system that is being configured. In most cases a
“FAIL” message will appear if incompatible selections are made by the
user, however it is the users responsibility to ensure proper
configuration of the unit.

**IMPORTANT**

Unit orientation selections must conform to the right hand coordinate
system as noted in Section `3.1 <\l>`__ of this user manual. Selecting
orientations that do not conform to this criteria are not allowed.

|UnitConfig|

Figure 24 Unit Configuration

Advanced Configuration
----------------------

Users who wish to access some of the more advanced features of NAV-VIEW
and the DMUx81ZA Series products can do so by selecting the “Advanced”
tab at the top of the “Unit Configuration” window.

|warning1| **WARNING**

Users are strongly encouraged to read and thoroughly understand the
consequences of altering the settings in the “Advanced” tab before
making changes to the unit configuration. These settings are discussed
in detail in Chapter 4 below.

Behavior switches are identified at the top of the page with marked
boxes. A blue box will appear if a switch has been enabled similar to
`Figure 25 <\l>`__ below. The values can be set in the same manner as
noted in the previous section. To set a value, users select the
appropriate “Modify” checkbox on the left side of the menu and select or
enable the appropriate value they wish to set. At the bottom of the
page, users have the option of temporarily or permanently setting
values. When all selections have been finalized, simply press the “Set
Values” button to change the selected settings.

|advanced|

Figure 25 Advanced Settings

Bit Configuration
-----------------

The third and final tab of the unit configuration window is “Bit
Configuration” (See `Figure 26 <\l>`__). This tab allows the users to
alter the logic of individual status flags that affect the masterStatus
flag in the master BITstatus field (available in most output packets).
By enabling individual status flags users can determine which flags are
logically OR’ed to generate the masterStatus flag. This gives the user
the flexibility to listen to certain indications that affect their
specific application. The masterFail and all error flags are not
configurable. These flags represent serious errors and should never be
ignored.

|BITConfig|

Figure 26 BIT Configuration

Mag Alignment Procedure
-----------------------

**IMPORTANT**

The following section only applies to DMUx81ZA Series products with
magnetometers (AHRS and INSx81ZA). If your particular model does not
utilize magnetometers for heading or performance you can disregard the
following section.

Hard Iron/Soft Iron Overview
----------------------------

The AHRS and INSx81ZA products use magnetic sensors to compute heading.
Ideally, the magnetic sensors would be measuring only earth’s magnetic
field to compute the heading angle. In the real world, however, residual
magnetism in your system adds to the total magnetic field measured. This
residual magnetism (called hard iron and soft iron) will create errors
in the heading measurement if it is not accounted for. In addition,
magnetic material can change the direction of the magnetic field as a
function of the input magnetic field. This dependence of the local
magnetic field on input direction is called the soft iron effect.

The AHRS and INSx81ZA products can actually measure the constant
magnetic field that is associated with your system and correct for it.
The AHRS and INSx81ZA products can also make a correction for some soft
iron effects. The process of measuring these non-ideal effects and
correcting for them is called the “Mag Alignment Procedure”. Performing
a “Mag Alignment Procedure” will help correct for magnetic fields that
are fixed with respect to the DMUx81ZA Series product. It cannot correct
for time varying fields, or fields created by ferrous material that
moves with respect to the DMUx81ZA Series product.

The AHRS and INSx81ZA products account for the extra magnetic field by
making a series of measurements, and using these measurements to model
the hard iron and soft iron environment in your system using a
two-dimensional algorithm. The AHRS and INSx81ZA products will calculate
the hard iron magnetic fields and soft iron corrections and store these
as calibration constants in the EEPROM.

The “Mag Alignment Procedure” should always be performed with the AHRS
or INSx81ZA product installed in the user system. If you perform the
calibration process with the DMUx81ZA Series product by itself, you will
not be correcting for the magnetism in the user system. If you then
install the DMUx81ZA Series product in the system (i.e. a vehicle), and
the vehicle is magnetic, you will still see errors arising from the
magnetism of the vehicle.

Mag Alignment Procedure Using NAV-VIEW
--------------------------------------

The Mag Alignment Procedure using NAV-VIEW can be performed using the
following steps below:

Select “Mag Alignment” from the “Configuration” drop down menu at the
top.

If you can complete your 360 degree turn within 120 seconds, select the
“Auto-Terminate” box.

Select the “Start” button to begin the “MagAlign” Procedure and follow
the instructions at the bottom of the screen as shown in `Figure
27 <\l>`__ below.

    |image29.png|

Figure 27 Mag Alignment

Rotate the AHRS or INSx81ZA product through x81 degrees of rotation or
until you receive a message to stop.

Once you have completed your rotation, you will be given data concerning
the calibration accuracy. The X and Y offset values indicate how far the
magnetic field has been offset due to hard iron affects from components
surrounding the unit. In addition, you will see a soft iron ratio
indicating the effect of soft iron on the AHRS of INSx81ZA product.

Save this data to the AHRS or INSx81ZA product by selecting the “Apply”
button (See `Figure 28 <\l>`__).

|MagComplete|

Figure 28 Magnetometer Alignment

Upon completion of the “Mag Alignment Procedure”, the heading accuracy
should be verified with all third party systems active using a known
reference such as a compass rose, GPS track or a calibrated compass.
Heading inaccuracies greater than the values specified on the data sheet
or fluctuating heading performance may be an indication of magnetic
field disturbances near the unit.

**IMPORTANT**

An acceptable calibration will provide X and Y Hard Iron Offset Values
of < 2.5 and a Soft Iron Ratio >0.95. If this procedure generates any
values larger than stated above, the system will assert the
softwareErrordataErrormagAlignOutOfBounds error flag. See section
`9 <\l>`__ for details on error flag handling. Note that the current
release of the software does not have this functionality. Future
releases of software will restore this functionality. The magnetometer
ranges is +/- 4 gauss, thus 2.5 gauss is the recommended maximum
hardiron that should be tolerated for the installation and still provide
ample resolution and headroom to properly determine the earth’s magnetic
field (strength < 0.5 gauss). If the hard iron estimates are larger than
2.5 gauss, then a different installation location should be
investigated. The hard iron and softiron data, while used internally to
achieve a heading reference, do not get applied to the magnetometer data
output in message A1 (see Section 7.4.3 and Section 8.3).

Read Unit Configuration
-----------------------

NAV-VIEW allows users to view the current settings and calibration data
for a given DMUx81ZA Series unit by accessing the “Read Configuration”
selection from the “Configuration” drop down menu (See `Figure
29 <\l>`__). From this dialog, users can print a copy of the unit’s
current configuration and calibration values with the click of a button.
Simply select the “Read” button at the top of the dialog box and upon
completion select the “Print” or “Print Preview” buttons to print a copy
to your local network printer. This information can be helpful when
storing hard copies of unit configuration, replicating the original data
sheet and for troubleshooting if you need to contact ACEINNA’s Support
Staff.

|ReadConfig|

Figure 29 Read Configuration

Firmware Upgrade
----------------

   Step 1, select Firmware upgrade from configuration menu. 

|upgrade1|

   
   Step 2, On pop-up window, select a new version binary file by clicking 
   SELECT button, then click Upgrade button.

|upgrade2|

|upgrade3|

   Step 3, wait for the process ongoing until a successful or failure message 
   pops up.

|upgrade4|

|upgrade5|


.. |warning1| image:: media/image2.jpeg
   :width: 0.25208in
   :height: 0.20903in
.. |LogFile| image:: media/image6.jpeg
   :width: 3.37361in
   :height: 2.68681in
.. |RawDataConsole| image:: media/image12.jpeg
   :width: 5.42639in
   :height: 3.88681in
.. |CompassView| image:: media/image13.jpeg
   :width: 5.52153in
   :height: 2.81736in
.. |PacketStatistics| image:: media/image14.jpeg
   :width: 3.05208in
   :height: 2.75625in
.. |UnitConfig| image:: media/image15.jpeg
   :width: 3.56528in
   :height: 5in
.. |advanced| image:: media/image16.jpeg
   :width: 3.38264in
   :height: 4.76528in
.. |BITConfig| image:: media/image17.jpeg
   :width: 3.65208in
   :height: 5.12153in
.. |image29.png| image:: media/image9.png
   :width: 4.81in
   :height: 6.07in
.. |MagComplete| image:: media/image18.jpeg
   :width: 4.51319in
   :height: 2.21736in
.. |ReadConfig| image:: media/image19.jpeg
   :width: 4in
   :height: 3.52153in
.. |upgrade1| image:: media/upgrade2.png
   :width: 3.66667in
   :height: 2.91042in
.. |upgrade2| image:: media/upgrade1.png
   :width: 3.66667in
   :height: 2.91042in
.. |upgrade3| image:: media/upgrade3.png
   :width: 3.66667in
   :height: 2.91042in
.. |upgrade4| image:: media/upgrade4.png
   :width: 3.66667in
   :height: 2.91042in
.. |upgrade5| image:: media/upgrade5.png
   :width: 3.66667in
   :height: 2.91042in
