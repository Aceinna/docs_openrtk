Aceinna Navigation Studio 
=========================

Aceinna Navigation Studio is a web-portal and UI for your OpenIMU.  To run it, first ensure the Python OpenIMU driver is installed, then 
start the server.  The server should report discovery and Connection to the OpenIMU.

Supported browsers are Chrome, Opera, and Edge.  Firefox also works but requires an extra step described here. https://stackoverflow.com/questions/11768221/firefox-websocket-security-issue 

Once this step is complete open the following link https://developers.aceinna.com/connect

The parameters that show up in this table are controlled by *openimu.json* and their corresponding code in *userConfiguration.c*.  Select the
packet that you would like to display.

To plot data go to the link https://developers.aceinna.com/record and click play. You can also log from this GUI.

Once a file is logged you can retrieve the file at https://developers.aceinna.com/files 

.. note::

    Your data file list is only shown to you and is tied to your login credentials.  The file list is not available to other users.





.. contents:: Contents
    :local:

