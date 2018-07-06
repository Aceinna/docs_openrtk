Installation of development tools and first steps
============================

.. contents:: Contents
    :local:
    

1. Platforms
--------------
OpenIMU development tools can run on different platforms:
 - Windows 10 or 7 PC
 - Ubuntu version 14.0+
 - Mac book
OpenIMU development tools are all open source tools.

2. Installation of OpenIMU development environment
---------------------
For developing on OpenIMU platform next set of tools needs to installed:

Visual Studio Code - can be downloaded from here: 

https://code.visualstudio.com

ST-link V2 driver (Windows only) - can be downloaded from here:  

http://www.st.com/en/development-tools/st-link-v2.html
 

3. Installation of OpenIMU development platform
-----------------------------------

To install OpenIMU development platform:

1. Start Visual Studio Code.
2. On leftmost toolbar find "Extensions" icon and click on it.
3. In the text box "Search extensions on Marketplace" type "Aceinna" and hit enter
4. Follow prompts.

4. First steps
-----------------------------------

After installation of "Aceinna" extension click on "Home" icon at the bottom of the screen. It will bring
up Aceinna OpenIMU platform homepage. Click on "Custom IMU examples", chose desired example and click "Import".

.. image:: media/HomePage.png  

The required example will be imported into working directory in folder:

C:\\Users\\<username>\\Documents\platformio\\Projects\\ProjectName (Windows)

Now you can edit, build and test the project. All your changes will remain in the above-mentioned directory and subdirectories.
Next time when you return to development - open Aceinna "Home" page and click "Open Project", choose "Projects" and select
required project from the list.

The source tree of imported project has next structure:

:: 

    project directory -|
                       |
                       |---build directory (.pioenvs)
                       |
                       |                                                               libraries
                       |---platform library directory (.piolibdeps)-|                     |
                       |                                            |--library name-|     V
                       |                                                            |---lib1--| 
                       |                                                            |         |--src  
                       |                                                            |         |--include   
                       |                                                            |            
                       |                                                            |            
                       |                                                            |---lib2--|  
                       |                                                            |         |--src  
                       |--include (user include files)                              |         |--include   
                       |                                                             ........            
                       |--lib (optional library directory) 				   
                       | 				   
                       |--src (user source files) 				   
                       | 				   
    
    
 


