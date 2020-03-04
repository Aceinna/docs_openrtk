Firmware Build from Source
=====================================

.. contents:: Contents
    :local:
    
System Setup
~~~~~~~~~~~~~~~~~~~
The following is a list of prerequisite software and hardware stack. 

* **Operating System Supported**

  - Windows 10 or 7
  - Linux (Ubuntu 14.0 or later)
  - MAC OS


* **Visual Studio Code and Aceinna Navigation Studio Extension**

  - Download and install Visual Studio Code (VS Code) from `here <https://code.visualstudio.com>`_
  - Intall "Aceinna Navigation Studio" extenion on VS Code

    1. Start Visual Studio Code, and on leftmost toolbar find "Extensions" icon and click on it.
    2. In the text box "Search extensions on Marketplace" type "Aceinna" and hit enter
    3. Install "Aceinna" Extension and Follow prompts.

    .. image:: media/AddExtension.png

      The "Aceinna" VS Code extension integrates the `PlatformIO <https://platformio.org/>`_ IDE that is a cross-platform new ecosystem for embedded development.


* **ST-LINK Debugger Driver**

  * *Mac OS* - comes with MAC OS
  * *Windows* - download and install from `here <http://www.st.com/en/development-tools/st-link-v2.html>`_
  * *Ubuntu* - as follows 

    * Clone the github repo on `Open Source STLink Tools <https://github.com/texane/stlink>`_ and read the instructions carefully.

    * Run the following commands to install ST-LINK V2 driver

        .. code:: bash

            # Run from source directory stlink/
            $ make Release
            $ cd build/Release
            $ sudo make install

            # Plug ST-LINK/V2 into USB, and check the device is present
            $ ls /dev/stlink-v2

* **Hardware**
 An OpenRTK330 EVK is required with a PC.

Import and Build Firmware from Source 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. **Open Aceinna Navigation Studio on VS Code**
   
  After installation of "Aceinna" extension, go to the lest most menu bar and click the "ant" icon of PlatformIO extension, it will bring up panel 1 and 2 (red numbered blue rectangle areas in the following figure) of PlatformIO, and then click "Quick Access->PIO Home->Open" will bring up the "Aceinna Navigation Studio" home page

    .. image:: media/AceinnaPlatformIOHome.jpg

  Alternatively, you can click the "Home" icon in blue rectangle #4 to bring up the home page.
  
2. **Import or Open Project** 

  The required example will be imported into working directory in folder:

    <PlatformIO Installaton Folder>/platformio/Projects/ProjectName

    Now you can edit, build and test the project. All your changes will remain in the above-mentioned directory and subdirectories.
    Next time when you return to development - open Aceinna "Home" page and click "Open Project", choose "Projects" and select
    required project from the list.

    The source tree of imported project tree has the following structure:

    ::

        project directory -|
                           |
                           |                                   
                           |--- .pio --|
                           |           |-- build --|   
                           |           |           |-- board-|   
                           |           |                     |-- binary image (firmware.bin)  
                           |           |                     |-- elf image (firmware.elf)  
                           |           |                     .  
                           |           |                     .  
                           |           |                     .  
                           |           |
                           |           |- libdeps -|   
                           |           |           |-- board-|  Library dependencies
                           |                                 |      
                           |                                 |--library1 src tree
                           |                                 |   
                           |                                 |--library2 src tree
                           |                                 |   
                           |                                 |--library3 src tree
                           |                                 |   
                           |                                 .  
                           |                                 .  
                           |                                            
                           |                                            
                           |--include (optional user include files)              
                           |                                            
                           |--lib (optional user library directory tree)
                           |
                           |--src (user source files tree)
                           |


3. **Compile and Load Firmware via JTAG**

 Once you have imported an example project, a good first step is to compile and download this application using your ST-LINK. At the bottom of the VS Code window is the shortcut toolbar shown below.  To load an application to the OpenRTK330 with JTAG, simply click the Install/Download button while the ST-LINK is connected to your EVB.

    .. image:: media/VSCodeToolBar.png
        :height: 200


The following contents of this section present the user APIs for each of the firmware options

* RAWDATA APP
* RTK APP
* RTK_INS APP

