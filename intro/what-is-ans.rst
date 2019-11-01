
What is Aceinna Navigation Studio?
==================================

.. image:: ../media/ANSHome.png

*   The Aceinna Navigation Studio (https://developers.aceinna.com) is a navigation system developer's website and web-platform.
*   It consists of a graphical user interface to control and configure OpenIMU units.
*   Using a JSON configuration file ("openimu.json"), the graphical user interface can be customized for user specific
    messaging and settings without any additional coding. This aligns the embedded code with both the Python device server
    and the GUI pages available on ANS (https://developers.aceinna.com).
*   Online tools include graphing, mapping, logging, and simulation.  
*   User Forum is available at (https://forum.aceinna.com).


**Python & the Acienna Navigation Studio**

The Acienna Navigation Studio (ANS) requires Python to operate.  If the user has not installed Python, it can be installed from
https://www.python.org/downloads/.  Download and install the latest version.

An open-source Python driver for OpenIMU is available and required.  The Python driver can be used directly from the terminal
to load, log, and test your application. The driver leverages the PySerial library to connect to an OpenIMU of a serial connection.  The python script supports configuring units, firmware updates (JTAG is faster for debugging), and local data logging.

In addition, the open-source Python driver can acts as a server connecting the OpenIMU hardware with our ANS developer platform for a GUI experience,
cloud data storage and retrieval, as well as stored file charting/plotting tools.

The Aceinna VS Code extension ensures a python environment automatically.  The OpenIMU python code can be installed independently by cloning the repository https://github.com/python-openimu or using pip as shown below.

.. code-block:: bash

    pip install openimu



.. contents:: Contents
    :local:

**Connection**

*   Connection Status is shown on the link symbol at the top right hand side of the page.
*   Device information is exposed on the main IMU page. (https://developers.aceinna.com/devices/record-next)
*   The baseline OpenIMU firmware provides a set of "standard settings" such as baud rate, output data rate, and more.
*   Custom options are added by adding additional options to "UserConfiguration" in *both* the OpenIMU embedded C code as
    well as the the openimu.json file which provides a summary of the descriptions and potential values for the UI.

.. image:: ../media/ANSIMU.png


**Graphing**

Use the play, record, and stop buttons to log data.

.. image:: ../media/ANSRecord.png

**File Retrieval**

Logged files are retrieved on the My Files page which opens up a zoomable graph view.
*Requires Login*

.. image:: ../media/ANSFiles.png
