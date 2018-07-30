************************************
Python-Based Serial-Message Decoder
************************************

.. contents:: Contents
    :local:
    
.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


Creating a python-based decoder
================================

The first step to using the OpenIMU decoder and spooling tools, *python-openimu*, to properly
decode an output message is to define the message in the file *openimu.json*. For the “e1”
message, the following is added to the file:

::

    {
        "name": "e1",
        "description": "VG_AHRS Output Message",
        "payload": [{
                "type": "uint32",
                "name": "timeCntr",
                "unit": "msec"
            },
            {
                "type": "double",
                "name": "time",
                "unit": "sec"
            },
            {
                "type": "float",
                "name": "roll",
                "unit": "rad"
            },
            {
                "type": "float",
                "name": "pitch",
                "unit": "rad"
            },
            {
                "type": "float",
                "name": "heading",
                "unit": "rad"
            },
            {
                "type": "float",
                "name": "xAccel",
                "unit": "g"
            },
            {
                "type": "float",
                "name": "yAccel",
                "unit": "g"
            },
            {
                "type": "float",
                "name": "zAccel",
                "unit": "g"
            },
            {
                "type": "float",
                "name": "xRate",
                "unit": "deg/s"
            },
            {
                "type": "float",
                "name": "yRate",
                "unit": "deg/s"
            },
            {
                "type": "float",
                "name": "zRate",
                "unit": "deg/s"
            },
            {
                "type": "float",
                "name": "xRateBias",
                "unit": "deg/s"
            },
            {
                "type": "float",
                "name": "yRateBias",
                "unit": "deg/s"
            },
            {
                "type": "float",
                "name": "zRateBias",
                "unit": "deg/s"
            },
            {
                "type": "float",
                "name": "xMag",
                "unit": "G"
            },
            {
                "type": "float",
                "name": "yMag",
                "unit": "G"
            },
            {
                "type": "float",
                "name": "zMag",
                "unit": "G"
            },
            {
                "type": "uint8_t",
                "name": "opMode",
                "unit": "N/A"
            },
            {
                "type": "uint8_t",
                "name": "linAccSw",
                "unit": "N/A"
            },
            {
                "type": "uint8_t",
                "name": "turnSw",
                "unit": "N/A"
            }
        ],
            "graphs": [{
                "name": "Acceleration",
                "units": "g",
                "xAxis": "Time (s)",
                "yAxes": ["xAccel", "yAccel", "zAccel"],
                "colors": ["#FF0000", "#00FF00", "#0000FF"],
                "yMax": 80
            },
            {
                "name": "Attitude (Roll/Pitch/Heading)",
                "units": "deg",
                "xAxis": "Time (s)",
                "yAxes": ["roll", "pitch", "heading"],
                "colors": ["#FF0000", "#00FF00", "#0000FF"],
                "yMax": 20
            },
            {
                "name": "Angular Rate",
                "units": "deg/s",
                "xAxis": "Time (s)",
                "yAxes": ["xRate", "yRate", "zRate"],
                "colors": ["#FF0000", "#00FF00", "#0000FF"],
                "yMax": 200
            },
            {
                "name": "Angular Rate-Bias Estimate",
                "units": "deg/s",
                "xAxis": "Time (s)",
                "yAxes": ["xRateBias", "yRateBias", "zRateBias"],
                "colors": ["#FF0000", "#00FF00", "#0000FF"],
                "yMax": 5
            },
            {
                "name": "Magnetic-Field",
                "units": "G",
                "xAxis": "Time (s)",
                "yAxes": ["xMag", "yMag", "zMag"],
                "colors": ["#FF0000", "#00FF00", "#0000FF"],
                "yMax": 5
            },
            {
                "name": "EKF Algorithm Flags",
                "units": "N/A",
                "xAxis": "Time (s)",
                "yAxes": ["opMode", "linAccSw", "turnSw"],
                "colors": ["#FF0000", "#00FF00", "#0000FF"],
                "yMax": 5
            }
        ]
    }


This information tells the decoder the order of the output data in the serial message, its type
(float, double, int, etc.), as well as the units associated with the data.  It also defines how the
data should be plotted, including axis-titles and colors.


.. note::

    A useful tool to check if the json-file is properly formatted is found at: https://jsonlint.com

