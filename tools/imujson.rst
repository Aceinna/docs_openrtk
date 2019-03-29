openimu.json Configuration File
===============================

The *openimu.json* file is used to describe the input and output messages and the configuration parameters of the OpenIMU.  An example
file is shown below.  The two sections that are edited during development are "userConfiguration" and "userMessages". These sections of
the JSON file correspond to equivalent sections of code in the your custom application.  The description provided in the *openimu.json* 
file is used by the Python driver to  support additional configuration parameters and messages that you add to your unit.  For example,
if you add a custom output message, the Python driver can automatically log it in a properly delimited CSV file format.  In addition,
the *openimu.json* file provides user friendly names and features that then appear in the ANS website automatically. Using the same custom
output message as an example, the *openimu.json* file can describe the graphs and plots that are shown on the "Record" page of the website.
The *openimu.json* file lets you reuse driver and UI code with little or no modification.

In the main OpenIMU source tree, you will find the "user" directory for your project.  This is where your custom IMU app code is integrated and built.  
The files *userConfiguration.h/userConfiguration.c* describes the various configuration parameters in the unit.  
The files *userMessaging.h/userMessaging.c* describes both the default and custom messages for your OpenIMU app.  These sections of c-code
are then described in the "userConfiguration" and "userMessages" section of in *openimu.json* as shown below.  If you add a new parameter in *userConfiguration.c*,
then you add a new parameter in "userConfiguration" following the examples.  Note each parameter must have a unique "paramId".  If you add 
a unique output message, you will add that both to the "Packet Type" options array, and as a new "outputPacket"  in "userMessages".  When adding
a new message the key point is to properly describe the payload in the order that the data is sent in *userMessaging.c*.  

.. code:: json

  {
    "name" : "OpenIMU300-EZ",
    "type" : "openimu",
    "description" : "9-axis OpenIMU with triaxial rate, acceleration, and magnetic measurement",
    "userConfiguration" : [
        { "paramId": 0, "paramType" : "disabled", "type" : "uint64", "name": "Data CRC"  },
        { "paramId": 1, "paramType" : "disabled", "type" : "uint64", "name": "Data Size" },
        { "paramId": 2, "paramType" : "select", "type" : "int64", "name": "Baud Rate", "options" : [38400, 57600, 115200]},
        { "paramId": 3, "paramType" : "select", "type" : "char8", "name": "Packet Type", "options" : ["z1", "zT"]},
        { "paramId": 4, "paramType" : "select", "type" : "int64", "name": "Packet Rate", "options" : [200, 100, 50, 20, 10, 0]},
        { "paramId": 5, "paramType" : "select", "type" : "int64", "name": "Accel LPF", "options" : [50, 25, 40, 20, 10, 5, 2]},
        { "paramId": 6, "paramType" : "select", "type" : "int64", "name": "Rate LPF", "options" : [50, 25, 40, 20, 10, 5, 2]},
        { "paramId": 7, "paramType" : "select", "type" : "char8", "name": "Orientation", "options" : ["+X+Y+Z"]}
    ],
    "userMessages" : { 
         "inputPackets" : [
              {
                "name" : "pG",
                "description" : "Get device serial number & factory ID",
                "inputPayload" : {
                },
                "responsePayload" : { 
                    "type" : "string",
                    "name" : "Device ID and SN"
                }
              },
              {
                "name" : "gV",
                "description" : "Get user app version",
                "inputPayload" : {},
                "responsePayload" : { 
                    "type" : "string",
                    "name" : "User Version"
                }
              },
              {
                "name" : "gA",
                "description" : "Get All Configuration Parameters",
                "inputPayload" : {},
                "responsePayload" : { 
                    "type" : "userConfiguration",
                    "name" : "Full Current Configuration"
                }
              },
              {
                "name" : "gP",
                "description" : "Get a Configuration Parameter",
                "inputPayload" : {
                    "type" : "paramId",
                    "name" : "Request Parameter Id"
                },
                "responsePayload" : { 
                    "type" : "userParameter",
                    "name" : "User Parameter"
                }
              },
              {
                "name" : "sC",
                "description" : "Save Configuration Parameters to Flash",
                "inputPayload" : {},
                "responsePayload" : {}
              },
              {
                "name" : "uP",
                "description" : "Update Configuration Parameter",
                "inputPayload" : {
                    "type" : "userParameter",
                    "name" : "Parameter to be Updated" 
                },
                "responsePayload" : {
                    "type" : "paramId",
                    "name" : "ID of the Updated Parameter"
                }
              }
         ],
         "outputPackets" : [
            {
                "name": "z1",
                "description": "Scaled 9-Axis IMU",	
                "payload" : [
                    {
                        "type" : "uint32",
                        "name" : "time",
                        "unit" : "s"
                    },
                    {
                        "type" : "float",
                        "name" : "xAccel",
                        "unit" : "G"
                    },
                    {
                        "type" : "float",
                        "name" : "yAccel",
                        "unit" : "G"
                    },
                    {
                        "type" : "float",
                        "name" : "zAccel",
                        "unit" : "G"
                    },
                    {
                        "type" : "float",
                        "name" : "xRate",
                        "unit" : "deg/s"
                    },
                    {
                        "type" : "float",
                        "name" : "yRate",
                        "unit" : "deg/s"
                    },
                    {
                        "type" : "float",
                        "name" : "zRate",
                        "unit" : "deg/s"
                    },
                    {
                        "type" : "float",
                        "name" : "xMag",
                        "unit" : "Gauss"
                    },
                    {
                        "type" : "float",
                        "name" : "yMag",
                        "unit" : "Gauss"
                    },
                    {
                        "type" : "float",
                        "name" : "zMag",
                        "unit" : "Gauss"
                    }
                ],
                "graphs" : [
                    { 
                        "name" : "Acceleration",
                        "units" : "m/s/s",
                        "xAxis" : "Time (s)",
                        "yAxes" : [ "xAccel", "yAccel", "zAccel"],
                        "colors" : [ "#FF0000", "#00FF00", "#0000FF" ],
                        "yMax" : 80
                    },
                    {
                        "name" : "Angular Rate",
                        "units" : "deg/s",
                        "xAxis" : "Time (s)",
                        "yAxes" : [ "xRate", "yRate", "zRate"],
                        "colors" : [ "#FF0000", "#00FF00", "#0000FF" ],
                        "yMax" : 400
                    }
                ]
            },
            {
                "name": "z2",
                "description": "Arbitrary type Values",	
                "payload" : [
                    {
                        "type" : "uint32",
                        "name" : "time",
                        "unit" : "s"
                    },
                    {
                        "type" : "uchar",
                        "name" : "c",
                        "unit" : ""
                    },
                    {
                        "type" : "int16",
                        "name" : "s",
                        "unit" : ""
                    },
                    {
                        "type" : "int32",
                        "name" : "i",
                        "unit" : ""
                    },
                    {
                        "type" : "int64",
                        "name" : "ll",
                        "unit" : ""
                    },
                    {
                        "type" : "double",
                        "name" : "d",
                        "unit" : ""
                    }
                ],
                "graphs" : [
                    {
                        "name" : "Angular Rate",
                        "units" : "deg/s",
                        "xAxis" : "Time (s)",
                        "yAxes" : [ "xRate", "yRate", "zRate"],
                        "colors" : [ "#FF0000", "#00FF00", "#0000FF" ],
                        "yMax" : 400
                    }
                ]
            },
            {
                "name": "z3",
                "description": "Scaled 6-Axis IMU Values",	
                "payload" : [
                    {
                        "type" : "int",
                        "name" : "timestamp",
                        "unit" : "ms"
                    },
                    {
                        "type" : "float",
                        "name" : "xAccel",
                        "unit" : "m/s/s"
                    },
                    {
                        "type" : "float",
                        "name" : "yAccel",
                        "unit" : "m/s/s"
                    },
                    {
                        "type" : "float",
                        "name" : "zAccel",
                        "unit" : "m/s/s"
                    },
                    {
                        "type" : "float",
                        "name" : "xRate",
                        "unit" : "rad/s"
                    },
                    {
                        "type" : "float",
                        "name" : "yRate",
                        "unit" : "rad/s"
                    },
                    {
                        "type" : "float",
                        "name" : "zRate",
                        "unit" : "rad/s"
                    }
                ],
                "graphs" : [
                    {
                        "name"   : "Acceleration",
                        "units"  : "m/s/s",
                        "xAxis"  : "timestamp (ms)",
                        "yAxes"  : [ "xRate", "yRate", "zRate"],
                        "colors" : [ "#FF0000", "#00FF00", "#0000FF" ],
                        "yMax"   : 100
                    }
                ]
            }
		]  
    },
    "bootloaderMessages": [
        {
            "name" : "JI",
            "description" : "Jump to Bootloader",
            "inputPayload" : {},
            "responsePayload" : {
                "type" : "ack",
                "response" : "Acknowledgement"
            }
        },
        {
            "name" : "JA",
            "description" : "Jump to App",
            "inputPayload" : {},
            "responsePayload" : {
                "type" : "none",
                "response" : "Empty"
            }
        },
        {
            "name" : "WA",
            "description" : "Write App Block",
            "inputPayload" : {
                "type" : "block",
                "name" : "4 byte block address followed by up to 240 bytes data"
            },
            "responsePayload" : {
                "type": "ack",
                "response" : "Acknowledgement"
            }
        }
    ]  
  }

.. note:: 

    Don't modify the "bootloaderMessages" section of *openimu.json*.  This section is used by the Python driver for the 
    in-system programming bootloader.  It should not be changed

.. contents:: Contents
    :local:

