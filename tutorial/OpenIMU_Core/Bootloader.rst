**********
Bootloader
**********

.. contents:: Contents
    :local:
    
.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


.. toctree::
    :maxdepth: 1


Each of the examples have its associated application pre-built as .bin files, which can be
downloaded directly onto the OpenIMU hardware directly from
`Aceinna Navigation Studio <https://developers.aceinna.com>`__.

**Download Procedure**
======================


*Connect to the OpenIMU Python Server*
--------------------------------------
From the terminal window, issue the command to start the OpenIMU Python Server:

.. _fig-term-python-server:

.. figure:: ./media/IMU_AppDownload_ServerConnect.PNG
    :alt: PythonServerConnect
    :width: 6.0in
    :align: center

    *Python Server Connection*


*Connect the Unit to the Aceinna Navigation Studio*
---------------------------------------------------

From the `Aceinna Navigation Studio <https://developers.aceinna.com>`__ main page, select *Code*
and *Apps* from the menu on the left-hand side of the window

.. _fig-term-app-page:

.. figure:: ./media/IMU_ApplicationPage.PNG
    :alt: ANS_AppPage
    :width: 7.0in
    :align: center

    *ANS Applications*



*Downloading the Application*
-----------------------------

Navigate to the desired application and click the *Download* link at the bottom of the application
box.  In this case, select the **IMU Application** *Download* link.

.. _fig-term-app-page:

.. figure:: ./media/IMU_App.png
    :alt: ANS_AppPage
    :width: 5.0in
    :align: center

    *IMU Application*


Once the *Download* link has been clicked, a progress bar at the top of the application box will
indicate how much time is left to download the application:

.. _fig-term-app-page:

.. figure:: ./media/IMU_AppProgressBar.png
    :alt: ANS_AppProgress
    :width: 5.0in
    :align: center

    *Application Download Progress*


*Download Progress View in s Terminal*
--------------------------------------

Additionally terminal messages in the window in which the Python server is running will indicate
progress.  Once complete, the terminal will indicate *Success* and *restart the app*.  At this
point the unit is now running the downloaded application.

.. _fig-term-app-page:

.. figure:: ./media/IMU_AppDownload_Progress.PNG
    :alt: ANS_ServerTerminalProgress
    :width: 5.0in
    :align: center

    *Terminal Download Progress Screen*


The unit can now be connected to the Navigation Studio and data plotted or saved in an output file.

